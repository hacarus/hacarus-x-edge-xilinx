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

* ホストマシン上のシリアルターミナルエミュレータ(Tera Term等)
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
Ubunts 16.04 | ...
Mac OS ... | ...
Windows 10 Home | Tera Term

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
* [SD_CARD]/video_cmd
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
    
[Linuxマシンをご利用の場合] 
    "ls -l /dev/ttyUSB*"で表示される、2番目のデバイスを指定する必要があります。例えば、/dev/ttyUSB0, /dev/ttyUSB1, /dev/ttyUSB2, /dev/ttyUSB3と表示される場合、/dev/ttyUSB1をPortに指定します。また、指定するポートに対してchmod 666によってアクセス権を変更する必要があります。
[Macマシンをご利用の場合] 
    "ls -l /dev/tty.*"で表示される、2番目のデバイスを指定する必要があります。例えば、/dev/tty.usbserial-000000, /dev/tty.usbserial-000001, /dev/tty.usbserial-000002, /dev/tty.usbserial-000003と表示される場合、/dev/tty.usbserial-000001をPortに指定します。
[Windowsマシンをご利用の場合]
    デバイスマネージャの「ポート(COMとLPT)」に表示される、2番目のデバイスを指定する必要があります。例えば、USB Serial Port(COM1), USB Serial Port(COM2), USB Serial Port(COM3), USB Serial Port(COM4)と表示される場合、COM2をPortに指定します。


### 4.2.2 Step 2 (実行)
1. シリアルターミナルにコマンドプロンプトが現れるまで待ちます。
2. コマンドプロンプトが現れたら、次のコマンドを送信し、micro SDカードディレクトリに移動します。

```
# cd /media/card
```
3.次の一連のコマンドを送信し、micro SDカード上の共有ライブラリをコピーします。
 
```
# cp lib/libopticalflow.so /usr/lib
# cp gstreamer-1.0/libgstsdxopticalflow.so /usr/lib/gstreamer-1.0
# cp lib/libgstsdxbase.so /usr/lib/gstreamer-1.0
# cp lib/libgstsdxallocator.so /usr/lib/gstreamer-1.0
```

4.次のコマンドを実行し、アプリケーションを実行します。

```
# gstdemo
```

<!--
# 5 ハードウェア構成を変更する場合

以下を記述予定(<span style="color:red;">PALTEK様のサポート希望</span>)

* DisplayPortを利用する場合
* HDMIの別のポートを利用する場合
* 別のUSBカメラを利用する場合
-->

---
# 5 参考資料

* [Hacarus Sparse AI Kit for FPGA ユーザマニュアル](https://hacarus.com/ja/fpga-kit/XXXX)
* [Xilinx Zynq UltraScale+ MPSoC ZCU104 キット](https://japan.xilinx.com/products/boards-and-kits/zcu104.html)
