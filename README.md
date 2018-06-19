# 64bit版 GNU Emacs 26.1 for Windows（w/ IMEパッチ）

GNU Emacs 26.1を、Windows向けに64bitでビルドしたものです。[公式のWindowsビルド](http://ftpmirror.gnu.org/emacs/windows/)相当に、いわゆるIMEパッチを当てています。よってストレス無く日本語入力できると思いますが、いきなり落ちたりする可能性もありますので、注意してお使いください。

ダウンロードは[こちら](https://github.com/mhatta/emacs-26-x86_64-win-ime/raw/master/emacs-26.1-x86_64-win-ime-20180619.zip)から。

## 実行方法

ダウンロードしたzipアーカイヴを展開し、`emacs-26.1\bin\runemacs.exe` を実行してください。

## ビルド方法

ビルド方法は基本的に[https://github.com/chuntaro/NTEmacs64](https://github.com/chuntaro/NTEmacs64)を踏襲しています。大体一式で3～4GB程度の空き容量が必要のようです。なお、当方は64bit版の Windows 10 Pro 1803（Build 17134.112）上でビルドしています。

### MSYS2のインストール

Windows向けUnix開発環境のMSYS2が必要です。[MSYS2 homepage](https://www.msys2.org/)から`msys2-x86_64-（日付）.exe`をダウンロードしてインストールしてください。64bit用ですので、i686ではなくx86_64のほうが必要です。

### 必要なパッケージのインストール

スタートメニューから「MSYS2 64bit」→「MSYS2 MinGW 64-bit」、あるいは`c:\msys64\mingw64.exe`を実行してMSYS2のシェルを起動します。MSYS2はArch Linuxのパッケージマネージャである[Pacman](https://wiki.archlinux.jp/index.php/Pacman)を採用していますので、すでにインストールされているパッケージを`$ pacman -Syu`でアップデートした上で、

```
$ pacman -S --needed base-devel \
  mingw-w64-x86_64-toolchain \
  mingw-w64-x86_64-xpm-nox \
  mingw-w64-x86_64-libtiff \
  mingw-w64-x86_64-giflib \
  mingw-w64-x86_64-libpng \
  mingw-w64-x86_64-libjpeg-turbo \
  mingw-w64-x86_64-librsvg \
  mingw-w64-x86_64-lcms2 \
  mingw-w64-x86_64-jansson \
  mingw-w64-x86_64-libxml2 \
  mingw-w64-x86_64-gnutls \
  mingw-w64-x86_64-zlib
```

を実行して開発に必要なパッケージを追加インストールします。Emacsのバージョンが上がるとたまに必要なパッケージが増えるので（たとえば26.1ではlcms2が追加された）、[公式のINSTALL.W64](https://git.savannah.gnu.org/cgit/emacs.git/tree/nt/INSTALL.W64)を適宜参照したほうがよいでしょう。

### Emacsのソースコードをダウンロード

MSYS2のシェルから`$ wget http://ftpmirror.gnu.org/emacs/emacs-26.1.tar.xz`を実行してEmacsのソースコードをダウンロードし、`$ tar xJf emacs-26.1.tar.xz`で展開します。`c:\` の直下に展開（`c:\emacs-26.1` のように）するのがよいでしょう。

### IMEパッチを当てる

このレポジトリから`emacs-26.1-windows-ime-（日付）.patch`をダウンロードし、

```
$ cd /c/emacs-26.1
$ patch -p1 < emacs-26.1-windows-ime-（日付）.patch
```

でパッチを当てます。その上で、`$ ./autogen.sh`を実行して`configure`スクリプトを更新してください。なお、このパッチは[rzl24oziさんが整理してくださったもの](https://gist.github.com/rzl24ozi/008d32c1f0742d3d2901295bf0366efa)をベースにしています。

### configureとmake

configure`を実行します。

```
$ CFLAGS='-O2 -march=x86-64 -mtune=generic -static -s -g0' LDFLAGS='-s' ./configure --prefix=/c/emacs-26.1 --without-dbus --without-compress-install --with-modules
```

[chuntaroさんのビルド](https://github.com/chuntaro/NTEmacs64)ではCFLAGSに`-Ofast -march=x86-64 -mtune=corei7`を与えてコンパイルしていますが、[このあたりの議論](https://www.reddit.com/r/emacs/comments/7gex1q/emacs_64bit_for_windows_with_imagemagick_7/)を見るとEmacsの場合は`-O2`のほうがパフォーマンスが良いらしいので、`-O2`に戻しました。

`configure`が終わったら、

```
$ make bootstrap; make install-strip
```

で`c:\emacs-26.1`以下にインストールされます。

### DLLのコピー

ビルドしたバイナリをMSYS2がインストールされていないマシンで使うには、MSYS2から必要なDLLを`c:\emacs-26.1\bin`以下にコピーして持っていく必要があります。このレポジトリにある `$ ./msys2-dll-copy.sh` を使うと良いでしょう。配付しているzipアーカイヴには必要なDLLだけを入れたつもりですが、抜けがあるかもしれませんので、その場合は自分でコピーしてください。DLLの依存関係に関しては、[Dependency Walker](http://www.dependencywalker.com/)を使うと分かります。
