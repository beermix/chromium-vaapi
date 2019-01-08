# Maintainer: Kien Dang Tran loganeast257@gmail.com
# Ex-Maintainer: Samantha McVey samantham@posteo.net
# Based off the official Chromium package, but with a patch to enable VA-API
# The VA-API patch is taken from the chromium-dev package source
# Official Arch Linux Chromium package Maintainers and Contributors:
#
# Maintainer: Evangelos Foutras <evangelos@foutrelis.com>
# Contributor: Pierre Schmitz <pierre@archlinux.de>
# Contributor: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>

pkgname=chromium-vaapi
pkgver=71.0.3578.98-2
pkgrel=6
_launcher_ver=6
pkgdesc="Chromium with VA-API support to enable hardware acceleration"
arch=('x86_64')
url="https://www.chromium.org/Home"
license=('BSD')
depends=('gtk2' 'nss' 'alsa-lib' 'libxss'  'libgcrypt'
         'systemd' 'dbus' 'json-glib'
         'libva')
provides=('chromium')
conflicts=('chromium')
makedepends=('python' 'python2' 'gperf' 'yasm' 'mesa' 'nodejs' 'git' 'clang' 'lld' 'minizip')
optdepends=('pepper-flash: support for Flash content'
            'kdialog: needed for file dialogs in KDE'
            'gnome-keyring: for storing passwords in GNOME keyring'
            'kwallet: for storing passwords in KWallet'
            'libva-intel-driver: support HW acceleration on Intel graphics cards'
            'libva-mesa-driver: support HW acceleration on AMD graphics cards'
            'libva-vdpau-driver-chromium: support HW acceleration on Nvidia graphics cards')
install=chromium.install
source=(https://commondatastorage.googleapis.com/chromium-browser-official/chromium-$pkgver.tar.xz
        chromium-launcher-$_launcher_ver.tar.gz::https://github.com/foutrelis/chromium-launcher/archive/v$_launcher_ver.tar.gz
        chromium-harfbuzz-r0.patch
        chromium-system-icu.patch
        chromium-widevine.patch
        chromium-skia-harmony.patch
        cfi-vaapi-fix.patch
        chromium-0002-allow-root.patch
        chromium-0003_oe-root-filesystem-is-readonly.patch
        chromium-vaapi-r21.patch
        chromium-58-glib.patch
        chromium-0002-Wall.patch
        default-allocator.patch
        define__libc_malloc.patch
        chromium-buildname.patch
        chromium-0013-march-westmere.patch
        notifications-nicer.patch
        title-bar-default-system.patch
        android.patch
as-needed.patch
attribute.patch
autocompletematch.patch
bootstrap.patch
empty-array.patch
fuzzers.patch
google-api-warning.patch
gpu-timeout.patch
gtk2.patch
inspector.patch
installer.patch
mojo.patch
openh264.patch
ownership-error.patch
parallel.patch
perfetto.patch
signin.patch
sizet.patch
swiftshader.patch
third-party-cookies.patch
unrar.patch
        widevine-other-locations.patch)

# Google API keys (see https://www.chromium.org/developers/how-tos/api-keys)
# Note: These are for Arch Linux use ONLY. For your own distribution, please
# get your own set of keys.
_google_api_key=AIzaSyDwr302FpOSkGRpLlUpPThNTDPbXcIn_FM
_google_default_client_id=413772536636.apps.googleusercontent.com
_google_default_client_secret=0ZChLK6AxeA3Isu96MkwqDR4

prepare() {
  cd "$srcdir/chromium-$pkgver"

  # Allow building against system libraries in official builds
  sed -i 's/OFFICIAL_BUILD/GOOGLE_CHROME_BUILD/' \
    tools/generate_shim_headers/generate_shim_headers.py

  # https://crbug.com/893950
  sed -i -e 's/\<xmlMalloc\>/malloc/' -e 's/\<xmlFree\>/free/' \
    third_party/blink/renderer/core/xml/*.cc \
    third_party/blink/renderer/core/xml/parser/xml_document_parser.cc \
    third_party/libxml/chromium/libxml_utils.cc

  # Load Widevine CDM if available
  patch -Np1 -i ../chromium-widevine.patch

  # https://crbug.com/skia/6663#c10
  patch -Np4 -i ../chromium-skia-harmony.patch

  # Fixes from Gentoo
  patch -Np1 -i ../chromium-harfbuzz-r0.patch

  # https://bugs.gentoo.org/661880#c21
  patch -Np1 -i ../chromium-system-icu.patch


  # Remove compiler flags not supported by our system clang
  sed -i \
    -e '/"-Wno-defaulted-function-deleted"/d' \
    build/config/compiler/BUILD.gn

  # Force script incompatible with Python 3 to use /usr/bin/python2
  sed -i '1s|python$|&2|' third_party/dom_distiller_js/protoc_plugins/*.py

  mkdir -p third_party/node/linux/node-linux-x64/bin
  ln -s /usr/bin/node third_party/node/linux/node-linux-x64/bin/

  msg2 'Applying VA-API patches'
  # patch -Np1 -i ../cfi-vaapi-fix.patch
  patch -Np1 -i ../chromium-vaapi-r21.patch

  msg2 'Applying OE patches'
  patch -Np1 -i ../chromium-0013-march-westmere.patch
  patch -Np1 -i ../chromium-0002-allow-root.patch
  patch -Np1 -i ../chromium-0003_oe-root-filesystem-is-readonly.patch
  #patch -Np1 -i ../notifications-nicer.patch
  #patch -Np1 -i ../unrar.patch
  #patch -Np1 -i ../title-bar-default-system.patch
  
  patch -Np1 -i ../android.patch
  patch -Np1 -i ../as-needed.patch
  patch -Np1 -i ../attribute.patch
  patch -Np1 -i ../autocompletematch.patch
  patch -Np1 -i ../bootstrap.patch
  patch -Np1 -i ../empty-array.patch
  patch -Np1 -i ../fuzzers.patch
  patch -Np1 -i ../google-api-warning.patch
  patch -Np1 -i ../gpu-timeout.patch
  patch -Np1 -i ../gtk2.patch
  patch -Np1 -i ../inspector.patch
  patch -Np1 -i ../installer.patch
  patch -Np1 -i ../mojo.patch
  patch -Np1 -i ../openh264.patch
  patch -Np1 -i ../ownership-error.patch
  patch -Np1 -i ../parallel.patch
  patch -Np1 -i ../perfetto.patch
  patch -Np1 -i ../signin.patch
  patch -Np1 -i ../sizet.patch
  patch -Np1 -i ../swiftshader.patch
  patch -Np1 -i ../third-party-cookies.patch
  patch -Np1 -i ../unrar.patch

  #patch -Np1 -i ../chromium-58-glib.patch

  #patch -Np1 -i ../default-allocator.patch
  #patch -Np1 -i ../define__libc_malloc.patch

  python2 build/linux/unbundle/replace_gn_files.py \
    --system-libraries "${!_system_libs[@]}"
}

build() {
  cd "$srcdir/chromium-$pkgver"

  export CCACHE_SLOPPINESS=time_macros

  export CC="ccache clang"
  export CXX="ccache clang++"
  export AR=ar
  export NM=nm

  local _flags=(
    'custom_toolchain="//build/toolchain/linux/unbundle:default"'
    'host_toolchain="//build/toolchain/linux/unbundle:default"'
    'clang_use_chrome_plugins=false'
    'is_official_build=true' # implies is_cfi=true on x86_64
    'treat_warnings_as_errors=false'
    'fieldtrial_testing_like_official_build=true'
    'ffmpeg_branding="Chrome"'
    'proprietary_codecs=true'
    'link_pulseaudio=false'
    'use_pulseaudio=false'
    'use_cups=false'
    'use_gnome_keyring=false'
    'use_sysroot=false'
    'linux_use_bundled_binutils=false'
    'use_custom_libcxx=false'
    'enable_hangout_services_extension=false'
    'enable_widevine=false'
    'enable_nacl=false'
    'enable_swiftshader=false'
    "google_api_key=\"${_google_api_key}\""
    "google_default_client_id=\"${_google_default_client_id}\""
    "google_default_client_secret=\"${_google_default_client_secret}\""
    'use_vaapi=true'
    'remove_webcore_debug_symbols=true'
    'use_gold=false'
    'use_kerberos=false'
    'is_debug=false'
    'enable_vr=false'
    'enable_vulkan=false'
    'is_desktop_linux=true'
    'use_dbus=true'
    'use_system_zlib=true'
    'use_system_freetype=true'
    'use_system_libdrm=true'
    'use_system_libpng=false'
    'use_system_harfbuzz=true'
    'use_system_libjpeg=true'
    'use_gio=true'
    'use_libpci=true'
    'icu_use_data_file=false'
    'enable_remoting=false'
    ~/.aa/chromium/chromium-vaapi
    'enable_wayland_server=false'
  )

  # Facilitate deterministic builds (taken from build/config/compiler/BUILD.gn)
  CFLAGS+='   -Wno-builtin-macro-redefined'
  CXXFLAGS+=' -Wno-builtin-macro-redefined'
  CPPFLAGS+=' -D__DATE__=  -D__TIME__=  -D__TIMESTAMP__='

  _flags+=('symbol_level=0')

  # Mimic exclude_unwind_tables=true
  CFLAGS+='   -fno-unwind-tables -fno-asynchronous-unwind-tables'
  CXXFLAGS+=' -fno-unwind-tables -fno-asynchronous-unwind-tables'
  CPPFLAGS+=' -DNO_UNWIND_TABLES'

  gn gen out/Release --args="${_flags[*]}" --script-executable=/usr/bin/python2

  ionice -c3 nice -n20 noti ninja -C out/Release chrome chrome_sandbox
}

package() {
  cd "$srcdir/chromium-$pkgver"

  install -D out/Release/chrome "$pkgdir/usr/lib/chromium/chromium"
  install -Dm4755 out/Release/chrome_sandbox "$pkgdir/usr/lib/chromium/chrome-sandbox"

  cp \
    out/Release/{chrome_{100,200}_percent,resources}.pak \
    out/Release/*.bin \
    "$pkgdir/usr/lib/chromium/"
  install -Dm644 -t "$pkgdir/usr/lib/chromium/locales" out/Release/locales/*.pak

  if [[ -z ${_system_libs[icu]+set} ]]; then
    cp out/Release/icudtl.dat "$pkgdir/usr/lib/chromium/"
  fi

  mv $pkgdir/usr/lib/chromium/chromium $pkgdir/usr/lib/chromium/chrome
}

# vim:set ts=2 sw=2 et:
