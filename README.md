# DSP-Radio-KT0937

## 概要
モジュールを制御するマイコンとのインターフェース用の端子配列は、M5StickC-PLUSのコネクタに直刺しで動作させることを考慮した設計にしています。
また、FM受信アンテナは、Φ3.5mmステレオミニジャックに接続するイアホンやオーディオケーブルのGNDラインを利用する仕様となっています。

## 選局/周波数変更
受信チャンネルの選局方法については、モジュールの４番端子をLowまたはHighにすることで周波数を上下に可変して行います。

![chnanel](https://github.com/massaJPN/DSP-Radio-KT0937/assets/44919621/4d05c0ba-7633-473d-a170-b9ee0dcbe70a)

## 音量調節
音量の調節については、可変抵抗器などの物理的なボリュームは実装していませんが、I2C制御でICの音量に関するレジスタ設定値を変更することで可変できます。

#### Arduino言語の例

  //smute gain: set to -36dB

  Wire.beginTransmission(DEVICE_ADDRESS);
  
  Wire.write(0xE3);      // Volume control
  
  Wire.write(0x02);      // 0x00 Mute 0x01-0x80 20log(SMUTE_GAIN/128)
  
  Wire.endTransmission();
  
  delay(100);

## その他
024年3月現在においては、KT0937-D8は電子部品ショップの秋月電子さんでも販売されていますので、そちらのWEBサイトにデータシートがありますのでご参照下さい。

モジュールに実装されている水晶振動子は、デフォルトのレジスタ設定で動作するように32.768KHzを使用しています。

## 外観

![gaikan60](https://github.com/massaJPN/DSP-Radio-KT0937/assets/44919621/da180994-a73f-47a3-a998-6808bbff6079)

## 端子配列
|Pin|Pin Name|I/O|Function|
|-----|-----|-----|-----|
|1|GND|Ground|Ground|
|2|NC|-|Non Connection|
|3|SCL|Digital I/O|SCL of 2-wire interface|
|4|INT|Analog Input|INput tuning Trigger (※connected to pin8 of KT0937-B8)|
|5|SDA|Digital I/O|SDA of 2-wire interface|
|6|NC|-|Non Connection|
|7|+3.3V|Power|Power supply (※2.1～3.6V)|
|8|NC|-|Non Connection|

## 使用例

### Arduino Uno R3 でコントロール

電源電圧に接続されたタクトスイッチを押して４番端子をHighにする事で100KHz周波数がダウンし同様にGNDに接続されたタクトスイッチを押して４番端子をLowにする事で100KHz周波数がアップします。
可変周波数範囲はICのレジスタ設定で決定します。（ICの機能としては32MHz～110MHzの受信が可能）

![arduino-example](https://github.com/massaJPN/DSP-Radio-KT0937/assets/44919621/91e56b5d-94c0-4fa4-9f99-23689d023d72)

### M5stickC Plus でコントロール

プログラミング次第ですが、周波数変更の手段としてM5stickのボタンAとボタンBを使用できます。
あらかじめ受信可能なラジオ曲の周波数をプリセットとしてプログラムに配列で定義すれば、ボタンAを押すとプリセットされた次の局へ進み、ボタンBで前の局へ戻るようなプログラムの実装が可能です。

![M5stickCPLUS-example](https://github.com/massaJPN/DSP-Radio-KT0937/assets/44919621/5642593c-ec4d-4da8-9249-e5ab40ef374f)

以上です。
