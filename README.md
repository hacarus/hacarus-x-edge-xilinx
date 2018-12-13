# Hacarus Sparse AI Kit for FPGA – テクニカル・ドキュメント(日本語版)


# 1. はじめに

Hacarus Sparse AI Kit for FPGA (以下、本キット)は、エッジで高速かつ低消費電力な学習および推論が実行可能な人工知能のスターターキットです。

本キットは、Xilinx ZCU104評価ボードとSDカードにインストールされる一連のソフトウェアとAIハードウェア構成ファイルからなります。

## 1.1 リリースノート

- 2018/12/XX オンライン動体検知機能リリース

## 1.2 サポート

本キットをご購入されたお客様向けに、e-mailによるテクニカルサポートを提供しています。サポートが必要なお客様は、FPGAボードのシリアル番号を記載の上、<fpga-support@hacarus.com> までご連絡ください。

---
# 2. 概要

## 2.1 システム構成

本キットは、[Xilinx Zynq UltraScale+ MPSoC ZCU104 キット](https://japan.xilinx.com/products/boards-and-kits/zcu104.html)(以下、ZCU104評価キット)と、ZCU104評価キット上でハードウェアアクセラレーションされるHacarus独自の AIアルゴリズムで構成されます。ZCU104評価キットは、組み込みビジョンアプリケーションのために設計されたFPGAプラットフォームで、組み込みビジョンアプリケーションに必要な周辺機器・インターフェースを備えています。

![本キットの概要](https://i.imgur.com/MoRkhuR.png)

本キットを用いることで、USBカメラからのライブ画像ストリームを独自AIアルゴリズムでオンライン学習し、予測結果をモニタ出力することが可能です。シリアル通信を介してホストマシンに解析結果を送信することも可能です。


### 2.1.1 オンライン動体検知

オンライン動体検知では、入力にUSBカメラを、出力にHDMIモニタまたはDisplayPortモニタを利用します。このキットは、固定されたUSBカメラが捉えたライブ画像ストリームの中から、動体を検出・マーキングしてモニタに出力します。ファイル名の末尾が moving-object となっている zip ファイルをダウンロードすることで利用が可能です。

---
# 3. 動作環境

## 3.1 ハードウェア

本キットを動作させるためには、以下のハードウェアが必要です。

* ZCU104評価ボード
* マイクロUSBケーブル(ホストマシン-ZCU104ボード間のシリアル通信用)
* マイクロSDカード
* 以下のいずれかの解像度をサポートするDisplayPortまたはHDMI入力のモニタ
    * 3840x2160
    * 1920x1080
    * 1280x720
* DisplayPortケーブルまたはHDMIケーブル
* e-con Systems See3CAM_CU30_CLT_TC USBカメラ

## 3.2 ソフトウェア

* ホストマシン上のシリアルターミナルエミュレータ
* SDカード・インストール・ファイル
    * ダウンロードは本キットに同梱されるユーザガイドに記載のダウンロードサイトにアクセスしてください

## 3.3 ライセンス

本ライセンスは、本キットをご購入いただいた時点から適用されます。本ライセンスには、3ヶ月間のソフトウェアおよびハードウェアのサポートとソフトウェアアップデートが含まれます。

ご購入から3ヶ月以降のサポートおよびソフトウェアアップデートには、別途ご相談ください。

## 3.4 互換性

本キットは以下のハードウェア構成の条件で、動作検証済みです。以下以外の OS や USB カメラで稼働させる場合はサポート対象外となりますのでご了承ください。

* ホストマシン及びターミナルエミュレータ

OS| エミュレータ
------------- | -------------
Ubunts 16.04 | gnome-terminal
Mac OS  | Terminal.app
Windows 10 Home | PuTTY

* USB3カメラ

型| 解像度
------------- | -------------
e-con See3CAM_CU30 | 1920x1080

モニタは LG 社製 27UD58 (1920x1080) を HDMI 接続にて動作確認しています。これ以外の解像度や接続方式で問題が発生した場合はサポートまでお知らせください。

---
# 4. インストールおよび操作方法

## 4.1 ボードのセットアップ

![ボード概要](https://i.imgur.com/tDI7LXC.jpg)

1. 12V電源プラグをコネクタ③に接続します。
2. 出力モニタに接続されたHDMIケーブルをコネクタ⑦に接続、または、DisplayPortケーブルをコネクタ⑥に接続します。
3. micro USBケーブルをUSB-UARTコネクタ①に接続し、もう一方のコネクタをホストマシンのUSBポートに挿入します。
4. ダウンロードしたzipファイルをZIPユーティリティで解凍して、micro SDカードのルートディレクトリに一連のファイルをコピーします(*1)。ファイルの階層が次のようになっていることを確認してください。

* [SD_CARD]/gstreamer-1.0/libgstsdxmotiondetection.so
* [SD_CARD]/lib/libgstsdxallocator.so
* [SD_CARD]/lib/libgstsdxbase.so
* [SD_CARD]/lib/libmotiondetection.so
* [SD_CARD]/BOOT.BIN
* [SD_CARD]/gstdemo
* [SD_CARD]/image.ub
* [SD_CARD]/init.sh
* [SD_CARD]/run.sh
* [SD_CARD]/video_cmd
* [SD_CARD]/app.cfg
* [SD_CARD]/README.txt

(*1) micro SDカードはFATでフォーマットされている必要があります。

5.SDカードを、microSDスロット②に挿入します。
6.e-con See3CAM_CU30 USBカメラを、USBコネクタ➄に接続します。
7.ブートモードスイッチ⑧(SW6)が、(1,2,3,4)=(ON, OFF, OFF, OFF)になっていることを確認します。

## 4.2 アプリケーション[xyz]の実行

### 4.2.1 Step 1 (準備)
1. 4.1章の方法で、micro SDカードにファイルをコピーしてFPGAボードのスロットに差し込まれていること、および、FPGAボードが正しくセットアップされているか確認します。
2. FPGAボードの電源スイッチ④をONにして、FPGAボードを起動します。

* 正しくファイルがインストールされたmicro SDカードが挿入されている場合、micro SD スロットの横のLEDが赤色から緑色に変化し点灯します。LEDが緑色に点灯しない場合は、4章の方法で、正しくFPGAボードが設定されているか確認します。特にブートモードスイッチ⑧が(1,2,3,4)=(ON,OFF,OFF,OFF)になっているか確認し、また、micro SDカードのディレクトリ階層が正しいか確認します。
* リブートスイッチ(⑨の2つのスイッチの一方)を押すことで、Linuxシステムをリブートすることができます。実行中に、誤ってUSBカメラをコネクタから抜いてしまった場合等、必要に応じてシステムを再起動することができます。

3.ホストマシン上でシリアル・ターミナル・エミュレータを起動し以下の設定でFPGAボードに接続します。

項目 | 値
------------- | -------------
Port  | (*2)参照
Baud Rate | 115200
Data Bits | 8bit
Stop Bits | 1bit
Parity | None
Flow Control | None

   (*2) お使いのホストマシン環境により、ポート番号は異なります。
    
 * シリアルポートの確認方法
    * Linuxマシンをご利用の場合
    "ls -l /dev/ttyUSB*"で表示される、2番目のデバイスを指定する必要があります。例えば、/dev/ttyUSB0, /dev/ttyUSB1, /dev/ttyUSB2, /dev/ttyUSB3と表示される場合、/dev/ttyUSB1をPortに指定します。また、指定するポートに対してchmod 666によってアクセス権を変更する必要があります。
    * Macマシンをご利用の場合
    "ls -l /dev/tty.*"で表示される、2番目のデバイスを指定する必要があります。例えば、/dev/tty.usbserial-000000, /dev/tty.usbserial-000001, /dev/tty.usbserial-000002, /dev/tty.usbserial-000003と表示される場合、/dev/tty.usbserial-000001をPortに指定します。
    * Windowsマシンをご利用の場合
    デバイスマネージャの「ポート(COMとLPT)」に表示される、2番目のデバイスを指定する必要があります。例えば、USB Serial Port(COM1), USB Serial Port(COM2), USB Serial Port(COM3), USB Serial Port(COM4)と表示される場合、COM2をPortに指定します。

 * シリアルポートの接続方法（ポートが上記例の場合）
    * Linuxマシンをご利用の場合
    gnome-terminalを起動し、"screen /dev/ttyUSB1 115200" を入力することにより、ボーレート115200でシリアルポートに接続されます。
    接続を終了する場合は、screen の画面で [control]+[a] を押して、次に [k] を押します。画面左下に Really kill this window [y/n] と表示されるので、 [y] を押すことで終了します。
    * Macマシンをご利用の場合
    Terminal.appを起動し、"screen /dev/tty.usbserial-000000 115200" を入力することにより、ボーレート115200でシリアルポートに接続されます。
    接続を終了する場合は、screen の画面で [control]+[a] を押して、次に [k] を押します。画面左下に Really kill this window [y/n] と表示されるので、 [y] を押すことで終了します。
    * Windowsマシンをご利用の場合
    PuTTYを起動します。PuTTY Configuration で Connection typeで"Serial"を選択し、Serial lineを"COM2"、Speedを"115200"に設定してOpenをクリックすることで、ボーレート115200でシリアルポートと接続することができます。

### 4.2.2 Step 2 (実行)
1. シリアルターミナルにコマンドプロンプトが現れるまで待ちます。
2. コマンドプロンプトが現れたら、次のコマンドを送信し、micro SDカードディレクトリに移動します。

```
# cd /media/card
```

3.次のコマンドを送信し、micro SDカード上の共有ライブラリをコピーします。
 
```
# bash init.sh
```

4.次のコマンドを送信し、アプリケーションを実行します。

```
# bash run.sh
```

---
# 5 アプリケーションごとの設定

## 5.1 オンライン動体検知の設定項目

app.cfg ファイルに記載しているオンライン動体検知の設定項目は、ユーザが必要に応じて値を変更できます。
以下の項目に対して、最小値から最大値の範囲で設定してください。
なお、設定ファイル中の項目名そのもの (例: MD_THRESHOLD) を変更した場合は無視されますので、変更しないでください。
最小値、最大値の値はT.B.D

項目名| デフォルト値 |最小値 | 最大値 | 内容 
------------- |  ------------- | ------------- | ------------- | -------------
MD_THRESHOLD |  5 | 1 | 100 | 値が小さいほど動体検知の感度が強くなります
THRE_RECT_MIN_W |  5 | 1 | 画像サイズの最大幅 |  矩形最小幅(*3)
THRE_RECT_MIN_H |  5 | 1 | 画像サイズの最大高さ |  矩形最小高さ(*3)
RECT_BORDER_W |  3 | 1 | 10 | 矩形の幅
THRE_MIN_G | 0.002 | 0超過 | THRE_MAX_G未満 | 値が小さいほど矩形数が減ります
THRE_MAX_G | 0.2 | THRE_MIN_G超過 | 1 | 値が大きいほど矩形数が減ります
THRE_SUP_D | 0.01 | 0 | 1.00 | 値が小さいほど矩形数が減ります

(*3) 幅 = THRE_RECT_MIN_W, 高さ = THRE_RECT_MIN_H 未満のサイズの矩形は描画しません。


---
# 6 参考資料

* [Hacarus Sparse AI Kit for FPGA ユーザマニュアル](https://hacarus.com/ja/fpga-kit/XXXX)
* [Xilinx Zynq UltraScale+ MPSoC ZCU104 キット](https://japan.xilinx.com/products/boards-and-kits/zcu104.html)
