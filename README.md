# 64bit版Emacs 26.1（w/ IMEパッチ）

Emacs 26.1を、Windows向けに64bitでビルドしたものです。[公式のWindowsビルド](http://ftpmirror.gnu.org/emacs/windows/)よりもコンパイラの最適化を強め（-Ofast -mmarch=x86-64 -mtune=corei7）、さらにいわゆるIMEパッチを当てています。よってストレス無く日本語入力できると思いますが、いきなり落ちたりする可能性もありますので、注意してお使いください。

ビルド方法は[https://github.com/chuntaro/NTEmacs64](https://github.com/chuntaro/NTEmacs64)を踏襲しています。なお、このビルドではSVGもサポートしています。
