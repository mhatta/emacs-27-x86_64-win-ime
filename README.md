# 64bit版Emacs 26.1 for Windows（w/ IMEパッチ）

Emacs 26.1を、Windows向けに64bitでビルドしたものです。[公式のWindowsビルド](http://ftpmirror.gnu.org/emacs/windows/)よりもコンパイラの最適化を強め（-Ofast -mmarch=x86-64 -mtune=corei7）、さらにいわゆるIMEパッチを当てています。よってストレス無く日本語入力できると思いますが、いきなり落ちたりする可能性もありますので、注意してお使いください。

ダウンロードは[こちら](https://github.com/mhatta/emacs-26-x86_64-win-ime/raw/master/emacs-26.1-x86_64-win-ime-20180616.zip)から。

ビルド方法は[https://github.com/chuntaro/NTEmacs64](https://github.com/chuntaro/NTEmacs64)を踏襲しています。起動方法などもこちらのページを参照してください。なお、このビルドではSVGもサポートしています。
