
<!-- MarkdownTOC autolink="true" autoanchor="false" markdown_preview="" uri_encoding="false" bullets="-,+,*" remove_image="false" -->

- [基础工具](#基础工具)
  + [Terminal基础配置](#terminal基础配置)
  + [SSH免密](#ssh免密)
- [安装包下载链接](#安装包下载链接)
  + [配置 & 编译](#配置--编译)
  + [Python2 & Python3 源码构建](#python2--python3-源码构建)
  + [GnuPG](#gnupg)
  + [Python 模块安装](#python-模块安装)
  + [构建流程备份](#构建流程备份)

<!-- /MarkdownTOC -->


## 基础工具

```bash
# xcode 命令行工具
xcode-select --install
```

### Terminal基础配置
```bash
git clone git@github.com:robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
# git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh

ln -s ~/Library/Mobile\ Documents/com\~apple\~CloudDocs/Developer/macOS-libs/zshrc ~/.zshrc
cp ~/Library/Mobile\ Documents/com\~apple\~CloudDocs/Developer/macOS-libs/zsh_history ~/.zsh_history
```

### SSH免密
```bash
# 生成ssh key
ssh-keygen -t rsa -b 4096 -C "iamyakirchen@outlook.com"
# 远程免密登录
touch ~/.ssh/authorized_keys
chmod 700 ~/.ssh                    # drwx------
chmod 600 ~/.ssh/*                  # -rw------- 
```

## 安装包下载链接

+ [autoconf](https://www.gnu.org/software/autoconf/autoconf.html)
  - https://ftp.gnu.org/gnu/autoconf/
+ [automake](http://www.gnu.org/software/automake)
  - https://ftp.gnu.org/gnu/automake/
+ [libtool](https://www.gnu.org/software/libtool/)
  - http://mirrors.nju.edu.cn/gnu/libtool/
+ [zlib](http://www.zlib.net)
+ [XZ Utils](https://tukaani.org/xz)
+ [uuid](http://www.ossp.org/pkg/lib/uuid)
+ [pkg-config](https://www.freedesktop.org/wiki/Software/pkg-config)
+ [Docutils](http://docutils.sourceforge.net)
+ [Freetype](https://www.freetype.org)
  - https://download.savannah.gnu.org/releases/freetype/
+ [Libffi](https://github.com/libffi/libffi)
+ [LibreSSL](https://www.libressl.org)
+ [Ccache](https://ccache.samba.org/download.html)
+ [libsodium](https://download.libsodium.org/libsodium/releases/)
  - https://github.com/jedisct1/libsodium
+ [Python](https://www.python.org/downloads/source)
+ [gdbm](https://www.gnu.org.ua/software/gdbm/download.html)
+ [Readline](https://tiswww.cwru.edu/php/chet/readline/rltop.html)
+ [mercurial](https://www.mercurial-scm.org)
+ [Ruby](http://www.ruby-lang.org/en/downloads)
+ [nginx](http://nginx.org/en/download.html)
+ [pcre](http://www.pcre.org)
+ [ngx_cache_purge](http://labs.frickle.com/nginx_ngx_cache_purge)
  - https://github.com/FRiCKLE/ngx_cache_purge/
+ [libssh2](https://www.libssh2.org)
  - https://github.com/libssh2/libssh2
+ [wget](https://ftp.gnu.org/gnu/wget/)
+ [bison](https://ftp.gnu.org/gnu/bison/)
+ [jq JSON processor](https://github.com/stedolan/jq)

### 配置 & 编译

```bash
# pkg-config
./configure --prefix=${LOCAL} --with-internal-glib && make -j 12 && make install
# zlib
./configure --prefix=${LOCAL} && make -j 12 && make install
# xz
# ./configure --prefix=${LOCAL} && make -j 12 && make install
# autoconf
./configure --prefix=${LOCAL} && make -j 12 && make install
# automake
./configure --prefix=${LOCAL} && make -j 12 && make install
# libtool
./configure --prefix=${LOCAL} && make -j 12 && make install && \
    cd ${LOCAL}/bin && ln -s libtoolize glibtoolize && cd -
# libressl
./configure --prefix=${LOCAL} && make -j 12 && make install
# boost
export BOOST_ROOT=/Volumes/To/repos/boost # 引入环境变量
./bootstrap.sh --prefix=${BOOST_ROOT} --with-libraries=all && \
    ./b2 -j 12 && ./b2 -j 12 --prefix=${BOOST_ROOT} install # 默认安装在/usr/local目录下
# freetype
./configure --prefix=${LOCAL} --without-harfbuzz && \
    make -j 12 && make install
# libffi
git clone --depth 1 git@github.com:libffi/libffi.git
cd libffi
./autogen.sh && \
# python ./generate-darwin-source-and-headers.py && \
./configure --enable-debug \
    --disable-dependency-tracking \
    --enable-purify-safety \
    --prefix=${LOCAL} && \
    make -j 12 && make install
# 使用macOS自带Python2 
# Ccache
./configure --prefix=${LOCAL} && make -j 12 && make install
# wget
./configure --prefix=${LOCAL}/wget \
    --with-ssl=openssl \
    --with-openssl=yes \
    --with-gnu-ld=no \
    --with-libssl-prefix=/Users/yakir/local \
    --without-libgnutls-prefix && make -j 12 && make install
# bison
./configure --prefix=${LOCAL} && make -j 12 && make install
# jq
autoreconf -i && \
    ./configure --with-oniguruma=builtin --disable-maintainer-mode --prefix=${LOCAL} && \
    make LDFLAGS=-all-static -j 12 && make LDFLAGS=-all-static install
# ruby 
gcl --depth 1  https://github.com/ruby/ruby.git && \
    autoconf && \
    ./configure --prefix=${RUBY_HOME} && \
    make -j 12 && \
    make install && \
    gem install cocoapods

# libsodium 支持 shadowsocks chacha20
./configure --prefix=${LOCAL} && make -j 12 && make install
```

### Python2 & Python3 源码构建

```bash
# Readline
git clone --depth 1 git://git.savannah.gnu.org/readline.git
mkdir build && cd build && \
../configure --prefix=${LOCAL} --enable-shared --enable-static --with-purify --enable-FEATURE=yes && \
make -j 12 && make install
# python2
mkdir build && cd build && \
../configure --enable-shared --enable-optimizations --enable-unicode=ucs4  --prefix=${PY2_HOME} && \
make -j 12 && make install && python2 --version
# python2扩展包安装
curl -O https://bootstrap.pypa.io/get-pip.py
python2 get-pip.py
pip2 install requests
# pip2包检查更新
pip2 list --outdate

# Docutils
./python2 setup.py install

# python3 python3.6.6 依赖 libffi 3.2.1
mkdir build && cd build && \
../configure --enable-shared \
    --enable-ipv6 \
    --with-dtrace \
    --enable-optimizations \
    --prefix=${PY3_HOME} && \
    make -j 12 && make install && python3 --version
# 在python3中自带pip，pip3包检查更新
pip3 list --outdate
```

### GnuPG

[gettext](http://www.gnu.org/software/gettext/)  
[libgpg-error](https://gnupg.org/ftp/gcrypt/libgpg-error)  
[libgcrypt](https://gnupg.org/ftp/gcrypt/libgcrypt/)  
[libassuan](https://gnupg.org/ftp/gcrypt/libassuan/)  
[libksba](https://gnupg.org/ftp/gcrypt/libksba/)  
[npth](https://gnupg.org/ftp/gcrypt/npth/)  
[gnupg](https://www.gnupg.org)  
[pinentry](https://gnupg.org/ftp/gcrypt/pinentry/)

```bash
pkgs=(
    "libgpg-error-1.33.tar.bz2"
    "libgcrypt-1.8.4.tar.bz2"
    "libassuan-2.5.2.tar.bz2"
    "libksba-1.3.5.tar.bz2"
    "npth-1.6.tar.bz2"
    "gnupg-2.2.12.tar.bz2"
    "gettext-0.19.8.1.tar.xz"
    "pinentry-1.1.0.tar.bz2"
    "ntbtls-0.1.2.tar.bz2"
);

urls=(
    "https://gnupg.org/ftp/gcrypt/libgpg-error/${pkgs[1]}"
    "https://gnupg.org/ftp/gcrypt/libgcrypt/${pkgs[2]}"
    "https://gnupg.org/ftp/gcrypt/libassuan/${pkgs[3]}"
    "https://gnupg.org/ftp/gcrypt/libksba/${pkgs[4]}"
    "https://gnupg.org/ftp/gcrypt/npth/${pkgs[5]}"
    "https://gnupg.org/ftp/gcrypt/gnupg/${pkgs[6]}"
    "https://ftp.gnu.org/pub/gnu/gettext/${pkgs[7]}"
    "https://gnupg.org/ftp/gcrypt/pinentry/${pkgs[8]}"
    "https://gnupg.org/ftp/gcrypt/ntbtls/${pkgs[9]}"
);

for pkg in ${urls[@]}; do
    curl -OL ${pkg}
done;

for pkg in ${pkgs[@]}; do
    if [[ -f ${pkg} ]]; then
        if [[ 'gz' == ${pkg##*.} ]]; then
            tar -zxf ${pkg}
        elif [[ 'bz2' == ${pkg##*.} ]]; then
            tar -jxf ${pkg}
        elif [[ 'xz' == ${pkg##*.} ]]; then
            tar -Jxf ${pkg}
        fi
    fi
done;
```

```bash
# gettext
./configure --prefix=${LOCAL} --disable-java && make -j 12 && make install
# adns
# ./configure --prefix=${LOCAL} --disable-dynamic && make -j 12 && make install
# libgpg-error
./configure --prefix=${LOCAL} \
    --disable-dependency-tracking \
    --disable-silent-rules \
    --enable-static && \
    make -j 12 && \
    make install
# libgcrypt
./configure --disable-dependency-tracking \
    --disable-silent-rules \
    --enable-static \
    --prefix=${LOCAL} \
    --disable-asm \
    --disable-jent-support && \
    make -j 12 && \
    make install
# libksba
./configure --disable-dependency-tracking \
    --disable-silent-rules \
    --prefix=${LOCAL} && \
    make -j 12 && \
    make install
# libassuan
./configure --disable-dependency-tracking \
    --disable-silent-rules \
    --prefix=${LOCAL} \
    --enable-static && \
    make -j 12 && \
    make install
# npth
./configure --disable-dependency-tracking \
    --disable-silent-rules \
    --prefix=${LOCAL} && \
    make -j 12 && \
    make install
# pinentry
./configure --disable-dependency-tracking \
    --disable-silent-rules \
    --prefix=${LOCAL}/pinentry \
    --disable-pinentry-qt \
    --disable-pinentry-qt5 \
    --disable-pinentry-gnome3 \
    --disable-pinentry-tqt \
    --disable-pinentry-fltk \
    --enable-pinentry-tty \
    --disable-pinentry-gtk2 && \
    make -j 12 && \
    make install
# ntbtls
./configure --disable-dependency-tracking \
    --enable-maintainer-mode \
    --disable-silent-rules \
    --prefix=${LOCAL} \
    --disable-heartbeat-support && \
    make -j 12 && \
    make install
# gnupg
./configure --disable-dependency-tracking \
      --disable-silent-rules \
      --disable-gnutls \
      --disable-ntbtls \
      --prefix=${LOCAL}/gnupg \
      --enable-symcryptrun \
      --with-pinentry-pgm=/Users/yakir/local/pinentry/bin/pinentry \
      --enable-all-tests \
      --disable-sqlite \
      --disable-ccid-driver && make -j 12 && \
      make install
      
# gpg-agent --homedir /Users/yakir/.gnupg --use-standard-socket --daemon
# 安装完成之后执行
# gpgconf --kill gpg-agent && gpg-agent --use-standard-socket --pinentry-program ${LOCAL}/pinentry/bin/pinentry --daemon
# gpg --list-secret-keys --keyid-format LONG
# gpg -v --keyserver keyserver.ubuntu.com --send-keys
# dirmngr.conf
# hkp-cacert /Users/yakir/Think/sks-keyservers.netCA.pem
```

### Python 模块安装

```bash
pip2 install requests
pip2 install Mercurial
```

### Shadowsocks
+ Python
```bash
pip2 install https://github.com/shadowsocks/shadowsocks/archive/master.zip -U
```
+ Go [shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go)
```bash
# on server
go get github.com/shadowsocks/shadowsocks-go/cmd/shadowsocks-server
# on client
go get github.com/shadowsocks/shadowsocks-go/cmd/shadowsocks-local
```
+ Rust [shadowsocks-rust](https://github.com/shadowsocks/shadowsocks-rust)
```bash
cargo install shadowsocks-rust
```

### Textmate

#### 依赖
- ninja — build system similar to make
- ragel — state machine compiler
- boost — portable C++ source libraries
- sparsehash — A cache friendly hash_map
- multimarkdown — marked-up plain text compiler
- mercurial — distributed SCM system
- Cap’n Proto — serialization library
- LibreSSL - OpenBSD fork of OpenSSL

```bash
# Ragel sparsehash 
./configure --prefix=${LOCAL} && make -j6 && make insatall
```


### 构建流程备份

```bash
# mercurial
rm -rf build
python2 setup.py build && \
python2 setup.py install --prefix=${PYTHON2_HOME}
hg --version

# nginx
./configure --prefix=${NGINX}  \
    --sbin-path=${NGINX}/nginx  \
    --conf-path=${NGINX}/nginx.conf  \
    --pid-path=${NGINX}/nginx.pid \
    --with-threads \
    --with-http_v2_module \
    --with-http_ssl_module \
    --http-log-path=/Volumes/To/logs/nginx/access.log \
    --error-log-path=/Volumes/To/logs/nginx/error.log \
    --lock-path=/Volumes/To/logs/nginx/nginx.lock \
    --with-http_stub_status_module \
    --with-http_realip_module \
    --with-pcre=../pcre-8.42  \
    --with-zlib=../zlib-1.2.11  \
    --with-openssl=../libressl-2.8.3 \
    --add-module=../ngx_cache_purge-2.3 && \
    make -j 12 && make install
# 开启 & 关闭
sudo nginx
sudo nginx -s stop

# Little CMS & FriBidi & libass & JPEG 
./configure --prefix=${LOCAL} && make -j 12 && make install

# Berkeley DB
cd build_unix
../dist/configure --prefix=${LOCAL} \
    --enable-cxx \
    --enable-compat185 \
    --enable-sql \
    --enable-sql_codegen \
    --enable-dbm \
    --enable-stl \
    --enable-jdbc \
    --enable-java \
    --enable-server && make -j 12 && make install

# jack-audio-connection-kit & libxml2
./configure --prefix=${LOCAL} && make -j 12 && make install 

# libbluray
git clone --recursive http://git.videolan.org/git/libbluray.git
./bootstrap && ./configure --prefix=${LOCAL} && make -j 12 && make install 

# libcaca
curl -L caca.zoy.org/files/libcaca/libcaca-0.99.beta19.tar.gz -o libcaca-0.99.beta19.tar.gz
./bootstrap
# 替换文件python/Makefile.in中的`$(am__py_compile) --destdir "$(DESTDIR)"`为`$(am__py_compile) --destdir "$(cacadir)"`
./configure --prefix=${LOCAL} \
    --disable-dependency-tracking \
    --disable-x11 \
    --disable-java \
    --disable-doc \
    --disable-slang \
    --disable-java \
    --disable-csharp \
    --disable-ruby && \
    make -j 12 && make install

# libdvdnav

# yasm
./autogen.sh # from source
./configure --prefix=${LOCAL} && make -j 12 && make install

# x265
hg clone https://bitbucket.org/multicoreware/x265
# cd x265 && hg co 1.2
cd build/linux
./make-Makefiles.bash
# `<enter>`  CMAKE_INSTALL_PREFIX   /Users/yakir/Developer/local
# `<g>`
make -j 12 && make install

# x264
./configure --prefix=${LOCAL} \
    --enable-shared \
    --enable-static \
    --enable-strip \
    --bit-depth=10 \
    --disable-lsmash

# Libav
./configure --prefix=${LOCAL} \
    --enable-shared \
    --enable-runtime-cpudetect \
    --disable-debug \
    --enable-gpl \
    --enable-version3 \
    --enable-libx264 \
    --enable-libx265 && \
    make -j 12 && \
    make install

# ffmpeg
./configure --prefix=${LOCAL} \
    --enable-shared \
    --enable-pthreads \
    --enable-gpl \
    --enable-version3 \
    --enable-hardcoded-tables \
    --enable-avresample \
    --enable-libx264 \
    --enable-lzma \
    --enable-libbluray && \
    make -j 12 && \
    make install

# MuJS
make prefix=/Users/yakir/Developer/local install

# mpv
python3 ./bootstrap.py 
python3 ./waf configure \
    --color=yes \
    --prefix=/Users/yakir/Developer/server/mpv \
    --enable-zsh-comp \
    --enable-libmpv-static \
    --enable-html-build \
    --enable-static-build \
    --enable-libbluray \
    --disable-x11 && \
    python3 ./waf build && \
    python3 TOOLS/osxbundle.py build/mpv
```

---
