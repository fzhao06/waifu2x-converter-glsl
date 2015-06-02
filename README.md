 waifu2x-converter-glsl
================================================================================
作成：うえした(@ueshita)

waifu2x-converter-cpp (https://github.com/WL-Amigo/waifu2x-converter-cpp) をベースに
OpenGLを使いGPUで計算処理を行うように改造したものです。
ノートPCの非力なIntel製GPUでもたぶん動きます。

コマンドライン周りはwaifu2x-converter.exeのままですので、
waifu2x-converter-glsl.exeをwaifu2x-converter.exeにリネームすれば
GUIフロントエンドがほぼそのまま使えると思います。

exeバイナリのダウンロードはこちら↓  
https://github.com/ueshita/waifu2x-converter-glsl/releases

 動作環境
----------

OpenGL 3.1が動作すること。  
（Intel HD Graphics 5000で動作確認済）


 使い方
--------

本ソフトはコマンドラインツールです。
`コマンドプロンプト` を立ち上げ、次のようにコマンドを打ち込み、使用して下さい。

以下のコマンドは、使い方を画面に出力します。
```
waifu2x-converter.exe --help
```

以下のコマンドは、画像変換を実行するコマンドの例です。
```
waifu2x-converter.exe -i C:\Users\amigo\Pictures\mywaifu.png -m noise_scale -j 8 --scale_ratio 1.6 --noise_level 2
```
以上を実行すると、`C:\Users\amigo\Pictures\mywaifu(noise_scale)(Level2)(x1.600000).png`に変換結果が保存されます。

本ソフトでは、以下のオプションを指定することが出来ます。

   -i <文字列>,  --input_file <文字列>
     (必須)  変換する画像へのパス(フルパスでの入力をおすすめします)

   -o <string>,  --output_file <string>
     変換された画像を保存するファイルへのパス(フルパスでの入力をおすすめします)
     拡張子(最後の.pngなど)は必ず入力するようにして下さい。
     指定しなかった場合は自動でファイル名を決定し、そのファイルに保存します。
     ファイル名の決定ルールは、
     `[元の画像ファイル名]``(モード名)``(ノイズ除去レベル(ノイズ除去モードの場合))``(拡大率(拡大モードの場合))``.png`
     のようになっています。
     保存される場所は、基本的には入力画像と同じディレクトリになります。
     (入力画像へのパスを相対パスで記述した場合、予期せぬことが起こる可能性があります)

   -m <noise|scale|noise_scale>,  --mode <noise|scale|noise_scale>
     変換モードを指定します。指定しなかった場合は`noise_scale`が選択されます。
      * noise : ノイズ除去を行います (正確には、ノイズ除去用のモデルを用いて画像変換を行います)
      * scale : 拡大を行います (正確には、既存アルゴリズムで拡大した後に、拡大画像補完用のモデルを用いて画像変換を行います)
      * noise_scale : ノイズ除去と拡大を行います (ノイズ除去を行った後に、引き続き拡大処理を行います)

   -b <整数値>,  --block_size <整数値>
     プログラム内で処理を分割するための基準となるブロックサイズを指定します。
     このオプションで指定した数のスレッドがプログラム内で起動されます。
     この数値を増やすことで効率的に処理を行うことができますが、より多くのグラフィックスメモリが必要になります。
     必要なグラフィックスメモリのサイズの計算式は以下です。  
       必要なグラフィックスメモリ = (ブロックサイズ) ^ 2 * 4 * 256 + α  
     指定した数値で処理することが無理な場合はエラーが発生します。
     デフォルト値は`512`です。

   --scale_ratio <小数点付き数値>
     何倍に拡大するかを指定します。デフォルト値は`2.0`ですが、2.0倍以外も指定できます。
     2.0以外の数値を指定すると、次のような処理を行います。
      * まず、指定された倍率を必要十分にカバーするように、2倍拡大を繰り返し行います。
      * 2の累乗以外の数値がしてされている場合は、指定倍率になるように拡大した画像を線形フィルタで縮小します。

   --noise_level <1|2>
     ノイズ除去レベルを指定します。ノイズ除去用のモデルはレベル1とレベル2のみ用意されているので、
      1 もしくは 2 を指定して下さい。
     デフォルト値は`1`です。

   --model_dir <文字列>
     モデルが格納されているディレクトリへのパスを指定します。デフォルト値は`models`です。
     基本的には指定しなくても大丈夫です。独自のモデルを使用する時などに指定して下さい。

   --,  --ignore_rest
     このオプションがしてされた後の全てのオプションを無視します。
     スクリプト・バッチファイル用です。

   --version
     バージョン情報を出力し、終了します。

   -h,  --help
     使い方を表示し、終了します。
     手軽に使い方を確認したい時などにどうぞ。


 更新履歴
----------

 * v1.0.0 : 初版
 * v1.1.0 : 
    - オリジナル版と比べて画像のまわりがにじんだように見える問題を修正
    - 低メモリ消費に抑えるための内部での画像分割処理を追加(出力は従来通りです)
    - 60%処理を高速化


 謝辞
------
本ソフトのオリジナル(C++版)から、使い方の説明等色々転載させていただきました。
C++版のWL-Amigo様、オリジナル版のultraist様に感謝いたします。


References
========================

- Original implementation: https://github.com/nagadomi/waifu2x
- Python implementation: https://marcan.st/transf/waifu2x.py
- C++ implementation: https://github.com/WL-Amigo/waifu2x-converter-cpp
