+++
date = "2016-07-10T21:31:08+09:00"
draft = false
title = "fr24feedを使ってflightrader24にADS-B情報をフィードする"
+++


## 概要

週末の工作として，Raspberry Pi 3 Model BでADS-B受信機を作成しました．と言っても半田ごてを持って電子工作などをするわけではなく，SDR受信機を購入してセットアップするだけの簡単なものです．ハードウェアの選定からソフトウェアのインストール方法まで，基本的に以下のブログを参考にさせていただきました．

[Raspberry Piで航空機からの位置情報信号ADS-Bを受信してみた - Okiraku Programming](http://d.hatena.ne.jp/NeoCat/20140402/1396406442)

その中で，取得したADS-Bの情報を[Flightradar24](https://www.flightradar24.com/)にフィードする部分で一部アップデートがあり，今ではWindowsを利用する必要がなくなりました．すべてRaspberry PiのCLI上で設定できるようになっていますので，その方法を以下にまとめておこうと思います．

![](/img/raspberry-pi.jpg)

## 環境

- <a href="http://www.amazon.co.jp/exec/obidos/ASIN/B01D3CXJNG/yagays-22/ref=nosim/" name="amazletlink" target="_blank">Raspberry Pi 3 Model B</a>
  - OSは公式の[Raspbian for Raspberry Pi](https://www.raspberrypi.org/downloads/raspbian/)を使用
  - Linux raspberrypi 4.4.14-v7+ #896 SMP Sat Jul 2 15:09:43 BST 2016 armv7l GNU/Linux
- <a href="http://www.amazon.co.jp/exec/obidos/ASIN/B00E0LAKRI/yagays-22/ref=nosim/" name="amazletlink" target="_blank">DVB-T+DAB+FM USB チューナー  RTL2832U+R820T</a>


## fr24feedのセッティング

### 1. 事前準備

事前に[Flightradar24.com](https://www.flightradar24.com/35.48,139.8/10)のアカウントを作成しておきます．flightrader24に情報提供すると有料のアカウントを割り当ててもらえるため，ここではアカウントを作るだけでPremium subscriptionに登録しないようにします．

また，Raspberry piの方も，rtl-sdrおよびdump1090のセッティングが完了しているものとします．


### 2. fr24feedのインストールとキーの取得

flightrader24へのデータの送信は，専用のfr24feedというソフトウェアを使用します．このソフトウェアは以下の2つの方法でインストールすることができます．

1. wgetでマニュアルでインストールする
2. apt-getを利用する

ここでは1のwgetを使ったインストールを行います．2の方法については[Forum](http://forum.flightradar24.com/threads/8908-New-Flightradar24-feeding-software-for-Raspberry-Pie?p=66479#post66479)の記事を参照ください．

```sh
$ sudo bash -c "$(wget -O - http://repo.feed.flightradar24.com/install_fr24_rpi.sh)"
```

上記のコマンドを実行するとfr24feedの初期設定が始まりますが，後から再度設定を行うので，ある程度適当で構いません．

ここで重要なのが，メールアドレスの登録です．この初期設定が終わると，アカウントに紐付いた16桁のキーが発行され，設定したメールアドレスに送られてきます．これとメールアドレスをセットでfr24feedに登録することで，情報をフィードすることができます．

![](/img/Flightradar24_sharing_key.png)

### 3. アカウントのセットアップ

先ほど取得したキーを含め，きちんとした情報をfr24feedに登録します．このとき，dump1090を起動しておきます(`dump1090 --net --aggressive --interactive`)．

`fr24feed --signup`コマンドを実行すると，以下の様な流れで適宜入力を求められるので，設定を入力していきます．

```sh
$  fr24feed --signup
```

まずメールアドレスを入力します．

```ini
Step 1.1 - Enter your email address (username@domain.tld)
$:example@example.com
```

次に，発行されたキーを入力します．

```ini
Step 1.2 - If you used to feed FR24 with ADS-B data before enter your sharing key.
If you don\'t remember your sharing key, pelase use the retrival form:
http://feed.flightradar24.com/forgotten_key.php

Otherwise leave this field empty and continue.
$:xxxxxxxxxxxxxxxx

Verifying sharing key...OK
```

MLATを利用するかどうか設定します．ここでは利用する設定にしています．

```ini
Step 1.3 - Would you like to participate in MLAT calculations? (yes/no)$:yes

IMPORTANT: For MLAT calculations the antenna's location should be entered very precise!
```

アンテナが設定されている場所の緯度経度および海抜高度を入力します．

- [座標を使用した検索と座標の取得 - パソコン - マップ ヘルプ](https://support.google.com/maps/answer/18539?source=gsearch&hl=ja)

```ini
Step 3.A - Enter antenna's latitude (DD.DDDD)
$:xx.xxxx

Step 3.B - Enter antenna's longitude (DDD.DDDD)
$:xxx.xxxx

Step 3.C - Enter antenna's altitude above the sea level (in feet)
$:xx

Using latitude: xx.xxxx, longitude: xx.xxxx, altitude: xxft above sea level
```

最後にdump1090とfr24feedが出力するログの設定をします．ここでdump1090を起動しておくと，自動的に検知して設定してくれます．あとは，ログの出力の方法と場所を指定して終わりです．

```ini
We have detected that you already have a dump1090 instance running. We can therefore automatically configure the FR24 feeder to use existing receiver configuration, or you can manually configure all the parameters.

Would you like to use autoconfig (*yes*/no)$:yes

Step 6A - Please select desired logfile mode:
 0 -  Disabled
 1 -  48 hour, 24h rotation
 2 -  72 hour, 24h rotation
 Select logfile mode (0-2)$:1

Step 6B - Please enter desired logfile path (/var/log):
$:/var/log

Submitting form data...OK

Congratulations! You are now registered and ready to share ADS-D data with Flightradar24.
+ Your radar id is T-RJTT135, please include it in all email communication with us.
+ Please make sure to start sharing data within the next 3 days as otherwise your ID/KEY will be deleted.

Thank you for supporting Flightradar24! We hope that you will enjoy our Premium services that will be available to you when you become an active feeder.

To start sending data now please execute:
sudo service fr24feed start

Saving settings to /etc/fr24feed.ini...OK
Settings saved, please run "sudo service fr24feed restart" to use new configuration.
```

これで完了です．あとは`sudo service fr24feed restart`でfr24feedを再起動します．


### 4. fr24feedが動作しているかの確認

最後に，fr24feedが正常に動作しているか確認をしておきます．`Active: active (running)`になっていれば問題ありません．

```sh
$ sudo service fr24feed status
● fr24feed.service - LSB: Flightradar24 Decoder & Feeder
   Loaded: loaded (/etc/init.d/fr24feed)
   Active: active (running) since Sun 2016-07-10 02:27:31 JST; 18h ago
  Process: 579 ExecStart=/etc/init.d/fr24feed start (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/fr24feed.service
           └─651 /usr/bin/fr24feed -- --monitor-file=/dev/shm/fr24feed.txt --write-pid=/var/run/fr24feed.pid --quiet --log-base=/home/pi/project/ads-b/log/fr24feed --log-rotat...

Jul 10 02:27:31 raspberrypi fr24feed[579]: Starting FR24 feeder: fr24feed.
Jul 10 02:27:31 raspberrypi systemd[1]: Started LSB: Flightradar24 Decoder & Feeder.
```

また，ローカルホストの8754ポートで立ち上がっているFR24 Feeder Statusを確認することでも確認することができます．

[http://localhost:8754](http://localhost:8754)

![](/img/fr24feed8754.png)

## Flightrader24.comで確認する

これでfr24feedを使ってADS-Bの情報をFlightrader24にフィードすることができました．最後に，Flightrader24の方でデータが受信できているか確認しましょう．一度Flightrader24のアカウントをログアウトするか少し時間を開けると，アカウントのページで"Your data sharing"と表示され，自身のRADAR IDが表示されていると思います．

やりました！これで晴れてFlightrader24のフィーダーとして認められ，Business相当のアカウント権限が付与されました．ウェブ上ですべての機能が使えるようになるほか，iPhone/iPadで480円で購入できるFlightradar24も無料で利用できるようになります．これは月\$49.99か年\$499.99相当のプランですので，なかなかの待遇っぷりです（データをフィードし続ける前提ではありますが）

[Flightradar24.com - Live flight tracker!](https://www.flightradar24.com/account)

![](/img/Flightradar24_account.png)

そして，自分が観測した機数や観測数，方角や距離などのフィードの統計を見ることもできます．最大で46nm(85km)の地点からの信号を受信できているようです(nmはナノメートルではなく海里です)．私の住んでいるところは周りに大きなマンションやビルが立ち並ぶ一角にあり，あまり電波の通りがよくないのでイマイチな結果ですが，見晴らしの良い高い場所にアンテナを置いたりアンテナ自体を工夫することで，この辺りは改善しそうですね．

![](/img/Flightradar24_stats1.png)

![](/img/Flightradar24_stats2.png)

## 参考

- [Share data with Flightradar24](http://feed.flightradar24.com/)
- [Raspberry Piで航空機からの位置情報信号ADS-Bを受信してみた - Okiraku Programming](http://d.hatena.ne.jp/NeoCat/20140402/1396406442)
- [Raspberry-PiでADS-Bを受信しFlightradar24にフィードする - Computer Radio RF Tech](http://ttrftech.tumblr.com/post/95416774356/receiving-ads-b-using-raspberry-pi-and-feed-flightradar2)
- [Updating flightradar24 with a Raspberry Pi](https://www.jacobtomlinson.co.uk/projects/2015/05/29/updating-flight-radar-24-with-a-raspberry-pi/)
- [５９からのんびりと・・・: ADS-B受信データをFlightradar24へ送ってみた（修正版）](http://blade8873.blogspot.jp/2015/12/ads-bflightradar24.html)
- [ラジオペンチ Raspberry PiでFlightradar24へデーターをフィード](http://radiopench.blog96.fc2.com/blog-entry-604.html)
