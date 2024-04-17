# TV-B-GONE V1.1 for ATMEL STUDIO 7.0
![TV-B-GONE 124058](https://github.com/todopapa/TV_B_GONE_11_AVR/assets/16860878/4fface5c-bb94-4956-87a0-1fbb0f91fc2d)

## はじめに
### TB-B-GONEとは
TV-BーGONEは、ほとんどすべてのテレビの電源を切ることが可能な小型のキーホルダー型ガジェットで、2004年にMitch 
Altman氏が発明しました。
当時アメリカではレストラン、バー、空港、コインランドリー、人々が集まるあらゆるところにテレビが置かれていた。Altman氏
によるとTV-B-Goneは、どこにでも存在して人の注意を奪い続けるテレビの呪縛から人々を解放するという意味あいを持って
いるらしい。
実際にこれを使うと、テレビの画面が突然消えるので、「あれっ、どうしたんだろう？」って慌てる人を見て楽しむプラクティカル
ジョーク的な楽しみの意味合いもあるように感じます。

### このプロジェクトの概要
TV-BーGONEはライセンスフリーで、Adafruit社が最初の[TV-BーGONEキット](https://www.adafruit.com/product/73)を出したのは2005年？前後で、8PINのAVRマイコン ATTINY85CPUを使って制御しています。ハードウェアとファームウェアは
当初のV1.0から、いまはV1.2に進化しています。
当初はNA版（北米/アジア）とEU版（ヨーロッパ版）にファームウェアが別れていましたが、V1.2では開きポートを抵抗一本で
（H/L）に制御することで、どちらのリージョンにも対応できるようになっています。
ただし、この時代にはAVRマイコンの開発環境として[WINAVR](https://winavr.sourceforge.net/)というAVR-GCCを
使ったCLIインターフェースで書かれており、今回は自分が良く使うATMEL STUDIOというIDE統合環境に移植しています。
ATMEL STUDIOに移行することで、ATMEL社（現MicroChip社）が用意した各種ライブラリが使えるので、変更修正が
容易になります。 （自分はこれをベースにおもちゃの修理用 IRリモコンのプラットフォームを開発しようとしています）
注意点として、V1.1のNA版が元のソースになっています。　必要な方は、EU版に置き換えるか、V1.2 World版に修正して下さい。
![TV-B-GONE_ORG](https://github.com/todopapa/TV_B_GONE_11_AVR/assets/16860878/55073afe-6188-482c-b98b-c53ae8d22c4e)
Adafruit TV-BーGONE V1.1　回路図

**TV_B_GONE_11_AVR**
は、AVRマイコン ATTNY85 のファームウェアです。

* ATTINY85 8pin DIP 対応
* ATMEL STUDIO（現Microchip Studio）対応
* 書き込みは AVR ISP MK２または、パラレルプログラマーTL866IIを使用する
* [Arduino as ISP](https://www.instructables.com/How-to-Program-an-Attiny85-From-an-Arduino-Uno/)
   Arduino Uno もArduino as ISPとして、ATTINY85の書き込み機としてつかえる

## 回路図

![TV=B=GPNE_V11_schema](https://github.com/todopapa/TV_B_GONE_11_AVR/assets/16860878/3f6f1ba9-0e45-4fa1-baad-bb48890133f6)

回路はAdafruitのV1.1をベースにしているので単純です。
ATTINY85の電源ピンにタクトSWを入れ、タクトSWを押したときにTINY85に電源が入り、ハードRESETと同様にプログラムが走ります。
写真はタクトSWが２つになっていますが、将来的に空きピンにタクトSWを入れれば、ON/OFF以外の他のコントロールもできます。
IR LEDは、PB0, PB1の２つを使ってますが、片方だけでもOKです。
IR LEDには直列に1Ωの抵抗をいれてますが、実験的には無くてもOKの様です。（Q1トランジスタのベース電流ｘHfeで流れる）

## ATMEL STUDIOプロジェクトファイル

頭のTV_B_GONE_11_AVR.atsln がプロジェクトファイルです。TV_B_GONE_11_AVRフォルダ内には
* main.c : メインプログラム
* NAcodes.c : 北米/アジア仕向け 各社テレビのON/OFF 赤外線コードの構造体
* util.c : デバッグ用ソフトUart (未使用）
* Debugフォルダ：生成したHEXファイル、ELFファイルが入る

## 使い方

タクトSWを押すとテレビのリモコンコードの発信が始まります。
各社リモコンコード発信の動作中は赤LEDが点滅して動作中を知らせます。
全部のコードが発振されるまで（テレビが消えるまで）押し続けてください。
全部発信されると、最後に4回LEDが短く点灯した後、消灯します。
動作が終わると、TINY85はSleepモードに入り、動作電流は < 1uA になります。
