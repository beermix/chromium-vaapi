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
pkgver=70.0.3538.77
pkgrel=5
_launcher_ver=6
pkgdesc="Chromium with VA-API support to enable hardware acceleration"
arch=('x86_64')
url="https://www.chromium.org/Home"
license=('BSD')
depends=('gtk2' 'nss' 'alsa-lib' 'xdg-utils' 'libxss'
         'ttf-font' 'systemd' 'dbus' 'pciutils' 'desktop-file-utils' 'hicolor-icon-theme' 'libva')
provides=('chromium')
conflicts=('chromium')
makedepends=('python' 'python2' 'gperf' 'yasm' 'mesa' 'nodejs' 'clang' 'lld')
optdepends=('pepper-flash: support for Flash content'
            'kdialog: needed for file dialogs in KDE'
            'gnome-keyring: for storing passwords in GNOME keyring'
            'kwallet: for storing passwords in KWallet'
            'libva-intel-driver: support HW acceleration on Intel graphics cards'
            'libva-mesa-driver: support HW acceleration on AMD graphics cards'
            'libva-vdpau-driver-chromium: support HW acceleration on Nvidia graphics cards')
install=chromium.install
source=(https://commondatastorage.googleapis.com/chromium-browser-official/chromium-$pkgver.tar.xz
        include-stdint.h-in-pdfium_mem_buffer_file_write.h.patch
        chromium-harfbuzz-r0.patch
        chromium-widevine-r2.patch
        chromium-skia-harmony.patch
        cfi-vaapi-fix.patch
        chromium-0002-allow-root.patch
        chromium-0003_oe-root-filesystem-is-readonly.patch
        chromium-70-gtk2.patch
        unrar.patch
        silencegcc.patch
        chromium-vaapi-r21.patch
        chromium-58-glib.patch
        chromium-59-gcc5.patch
        chromium-61-gcc5.patch
        chromium-62-gcc5.patch
        chromium-62-gcc7.patch
        chromium-64-gcc7.patch
        chromium-65-gcc7.patch
        chromium-66-gcc7.patch
        chromium-67-gcc7.patch
        chromium-68-gcc7.patch
        chromium-68-gcc8.patch
        chromium-69-cinnamon.patch
        chromium-69-gcc7-my-icu.patch
        chromium-69-gcc8.patch
        chromium-70-gcc8.patch
        chromium-compiler-r4.patch
        chromium-0002-Wall.patch
        chromium-gcc8-r588316.patch
        chromium-gcc8-r589614.patch
        fixes_mojo.patch
        libcxx.patch
        optimize.patch
        chromium-system-icu.patch
        vpx.patch
        default-allocator.patch
        define__libc_malloc.patch
        chromium-buildname.patch
        chromium-0013-march-westmere.patch)

# Possible replacements are listed in build/linux/unbundle/replace_gn_files.py
# Keys are the names in the above script; values are the dependencies in Arch
declare -gA _system_libs=(
  [fontconfig]=fontconfig
  [freetype]=freetype2
  [harfbuzz-ng]=harfbuzz
  #[icu]=icu
  [libdrm]=
  [libjpeg]=libjpeg
  #[libpng]=libpng            # https://crbug.com/752403#c10
  [libxml]=libxml2
  [libxslt]=libxslt
  [yasm]=
)
_unwanted_bundled_libs=(
  ${!_system_libs[@]}
  ${_system_libs[libjpeg]+libjpeg_turbo}
)
depends+=(${_system_libs[@]})

# Google API keys (see https://www.chromium.org/developers/how-tos/api-keys)
# Note: These are for Arch Linux use ONLY. For your own distribution, please
# get your own set of keys.
 _google_api_key=AIzaSyDwr302FpOSkGRpLlUpPThNTDPbXcIn_FM
 _google_default_client_id=413772536636.apps.googleusercontent.com
 _google_default_client_secret=0ZChLK6AxeA3Isu96MkwqDR4

#_google_api_key=AIzaSyAQ6L9vt9cnN4nM0weaa6Y38K4eyPvtKgI
#_google_default_client_id=740889307901-4bkm4e0udppnp1lradko85qsbnmkfq3b.apps.googleusercontent.com
#_google_default_client_secret=9TJlhL661hvShQub4cWhANXa

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

  # https://crbug.com/879900
  #patch -Np1 -i ../include-stdint.h-in-pdfium_mem_buffer_file_write.h.patch

  # https://crbug.com/skia/6663#c10
  patch -Np4 -i ../chromium-skia-harmony.patch

  # Fixes from Gentoo
  patch -Np1 -i ../chromium-harfbuzz-r0.patch
  patch -Np1 -i ../chromium-widevine-r2.patch

  patch -Np1 -i ../chromium-0002-allow-root.patch
  patch -Np1 -i ../chromium-0003_oe-root-filesystem-is-readonly.patch

  patch -Np1 -i ../chromium-70-gtk2.patch
  patch -Np1 -i ../chromium-58-glib.patch
  patch -Np1 -i ../chromium-59-gcc5.patch
  patch -Np1 -i ../chromium-61-gcc5.patch
  patch -Np1 -i ../chromium-62-gcc5.patch
  patch -Np1 -i ../chromium-62-gcc7.patch
  patch -Np1 -i ../chromium-64-gcc7.patch
  patch -Np1 -i ../chromium-65-gcc7.patch
  patch -Np1 -i ../chromium-66-gcc7.patch
  patch -Np1 -i ../chromium-67-gcc7.patch
  patch -Np1 -i ../chromium-68-gcc7.patch
  #patch -Np1 -i ../chromium-68-gcc8.patch
  patch -Np1 -i ../chromium-69-cinnamon.patch
  #patch -Np1 -i ../chromium-69-gcc7-my-icu.patch
  patch -Np1 -i ../chromium-69-gcc8.patch
  patch -Np1 -i ../chromium-70-gcc8.patch
  patch -Np1 -i ../chromium-compiler-r4.patch
  patch -Np1 -i ../chromium-0002-Wall.patch
  patch -Np1 -i ../silencegcc.patch
  patch -Np1 -i ../fixes_mojo.patch
  patch -Np1 -i ../libcxx.patch
  patch -Np1 -i ../optimize.patch
  patch -Np1 -i ../unrar.patch
  patch -Np1 -i ../chromium-system-icu.patch

  patch -Np1 -i ../vpx.patch
  patch -Np1 -i ../default-allocator.patch
  patch -Np1 -i ../define__libc_malloc.patch
  patch -Np1 -i ../chromium-buildname.patch
  patch -Np1 -i ../chromium-0013-march-westmere.patch

  patch -Np1 -i ../chromium-gcc8-r588316.patch
  #patch -Np1 -i ../chromium-gcc8-r589614.patch

  # Force script incompatible with Python 3 to use /usr/bin/python2
  sed -i '1s|python$|&2|' third_party/dom_distiller_js/protoc_plugins/*.py

  mkdir -p third_party/node/linux/node-linux-x64/bin
  ln -s /usr/bin/node third_party/node/linux/node-linux-x64/bin/

  # VA-API patch
  msg2 'Applying VA-API patches'
  patch -Np1 -i ../cfi-vaapi-fix.patch
  patch -Np1 -i ../chromium-vaapi-r21.patch

  # Remove bundled libraries for which we will use the system copies; this
  # *should* do what the remove_bundled_libraries.py script does, with the
  # added benefit of not having to list all the remaining libraries
  local _lib
  for _lib in ${_unwanted_bundled_libs[@]}; do
    find "third_party/$_lib" -type f \
      \! -path "third_party/$_lib/chromium/*" \
      \! -path "third_party/$_lib/google/*" \
      \! -path 'third_party/yasm/run_yasm.py' \
      \! -regex '.*\.\(gn\|gni\|isolate\)' \
      -delete
  done

  python2 build/linux/unbundle/replace_gn_files.py \
    --system-libraries "${!_system_libs[@]}"
}

build() {
  cd "$srcdir/chromium-$pkgver"

  export CCACHE_SLOPPINESS=time_macros

  export CC='ccache clang'
  export CXX='ccache clang++'
  export AR=ar
  export NM=nm

  local _flags=(
    'custom_toolchain="//build/toolchain/linux/unbundle:default"'
    'host_toolchain="//build/toolchain/linux/unbundle:default"'
    'clang_use_chrome_plugins=false'
    'is_official_build=true' # implies is_cfi=true on x86_64
    'treat_warnings_as_errors=false'
    'is_debug=false'
    'fieldtrial_testing_like_official_build=true'
    'ffmpeg_branding="Chrome"'
    'proprietary_codecs=true'
    'link_pulseaudio=false'
    'use_pulseaudio=false'
    'use_cups=false'
    'gtk_version=2'
    'use_gnome_keyring=false'
    'use_sysroot=false'
    'linux_use_bundled_binutils=false'
    'use_custom_libcxx=false'
    'enable_vulkan=false'
    'enable_hangout_services_extension=true'
    'enable_widevine=true'
    'enable_nacl=false'
    'enable_mdns=true'
    'enable_vr=false'
    'enable_wayland_server=false'
    'is_desktop_linux=true'
    'remove_webcore_debug_symbols=true'
    'use_allocator="none"'
    'use_alsa=true'
    'use_aura=true'
    'use_dbus=true'
    'use_gio=true'
    'use_glib=true'
    'use_libpci=true'
    'enable_remoting=false'
    'rtc_enable_protobuf=false'
    'enable_swiftshader=false'
    "google_api_key=\"${_google_api_key}\""
    "google_default_client_id=\"${_google_default_client_id}\""
    "google_default_client_secret=\"${_google_default_client_secret}\""
    'use_vaapi=true'
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

  #python2 third_party/libaddressinput/chromium/tools/update-strings.py

  ninja -j7 -C out/Release chrome chrome_sandbox
  #ionice -c3 nice -n20 
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

  #if [[ -z ${_system_libs[icu]+set} ]]; then
    cp out/Release/icudtl.dat "$pkgdir/usr/lib/chromium/"
  #fi
}
