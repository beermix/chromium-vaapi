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
pkgver=71.0.3578.127
pkgrel=105
pkgdesc="Chromium with VA-API support to enable hardware acceleration"
arch=('x86_64')
url="https://www.chromium.org/Home"
license=('BSD')
depends=('gtk2' 'nss' 'alsa-lib' 'systemd' 'dbus' 'json-glib' 'xdg-utils' 'libxss' 'libva' 'at-spi2-atk')
provides=('chromium')
conflicts=('chromium')
makedepends=('python' 'python2' 'gperf' 'yasm' 'mesa' 'nodejs' 'git' 'clang' 'minizip' 'fakeroot' 'bison' 'flex' 'nodejs')
optdepends=('pepper-flash: support for Flash content'
            'kdialog: needed for file dialogs in KDE'
            'gnome-keyring: for storing passwords in GNOME keyring'
            'kwallet: for storing passwords in KWallet'
            'libva-intel-driver: support HW acceleration on Intel graphics cards'
            'libva-mesa-driver: support HW acceleration on AMD graphics cards'
            'libva-vdpau-driver-chromium: support HW acceleration on Nvidia graphics cards')
install=chromium.install
source=(https://commondatastorage.googleapis.com/chromium-browser-official/chromium-$pkgver.tar.xz
       chromium-harfbuzz-r0.patch
        chromium-system-icu.patch
        chromium-widevine.patch
        chromium-skia-harmony.patch
        cfi-vaapi-fix.patch
        chromium-vaapi-r21.patch
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
flag-for-search-engine-collection.patch
flag-to-configure-extension-downloading.patch
flag-to-disable-beforeunload.patch
flag-to-enable-potentially-annoying-security-features.patch
flag-to-force-punycode-hostnames.patch
flag-to-hide-crashed-bubble.patch
flag-to-show-avatar-button.patch
flag-to-stack-tabs.patch
ipv6-probing-option.patch
suggestions-url-field.patch
third-party-ungoogled.patch
block-trk-and-subdomains.patch
clear-http-auth-cache-menu-item.patch
default-to-https-scheme.patch
disable-crash-reporter.patch
disable-domain-reliability.patch
disable-download-quarantine.patch
disable-fonts-googleapis-references.patch
disable-formatting-in-omnibox.patch
disable-gaia.patch
disable-gcm.patch
disable-google-host-detection.patch
disable-intranet-redirect-detector.patch
disable-mei-preload.patch
disable-network-time-tracker.patch
disable-profile-avatar-downloading.patch
disable-signin.patch
disable-translate.patch
disable-untraceable-urls.patch
disable-webgl-renderer-info.patch
disable-webrtc-log-uploader.patch
disable-webstore-urls.patch
enable-page-saving-on-more-pages.patch
fix-building-without-mdns-and-service-discovery.patch
fix-building-without-one-click-signin.patch
fix-building-without-safebrowsing.patch
fix-learn-doubleclick-hsts.patch
no-such-option-no-sysroot.patch
popups-to-tabs.patch
remove-disable-setuid-sandbox-as-bad-flag.patch
remove-fcomplete-member-pointers-cflag.patch
remove-third-party-analytics.patch
replace-google-search-engine-with-nosearch.patch
searx.patch
use-local-devtools-files.patch
fix-libva1-compatibility.patch
fix-nullptr-t-namespace.patch
widevine-other-locations.patch
alignof.patch
ambiguous-overloads.patch
as-needed.patch
autocompletematch.patch
chromedriver-revision.patch
constexpr.patch
constructor.patch
crc32.patch
empty-array.patch
namespace.patch
optimize.patch
polymer.patch
ps-print.patch
widevine.patch
widevine-revision.patch
icu.patch
fontconfig.patch
atk.patch
convertutf.patch
event.patch
bootstrap.patch
libcxx.patch
sysroot.patch
)

# Possible replacements are listed in build/linux/unbundle/replace_gn_files.py
# Keys are the names in the above script; values are the dependencies in Arch
declare -gA _system_libs=(
  [fontconfig]=fontconfig
  [freetype]=freetype2
  [harfbuzz-ng]=harfbuzz
  [icu]=icu
  [libdrm]=
  [libjpeg]=libjpeg
  [libxml]=libxml2
  [libxslt]=libxslt
  #[re2]=re2
  #[snappy]=snappy
  [yasm]=
  #[zlib]=minizip
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
  patch -Np1 -i ../chromium-vaapi-r21.patch

  msg2 'Applying OE patches'
  patch -Np1 -i ../chromium-0002-allow-root.patch
  patch -Np1 -i ../chromium-0003_oe-root-filesystem-is-readonly.patch
  #patch -Np1 -i ../notifications-nicer.patch
  #patch -Np1 -i ../title-bar-default-system.patch

  patch -Np1 -i ../fuzzers.patch
  patch -Np1 -i ../google-api-warning.patch
  patch -Np1 -i ../gpu-timeout.patch
  patch -Np1 -i ../gtk2.patch
  patch -Np1 -i ../inspector.patch
  patch -Np1 -i ../mojo.patch
  patch -Np1 -i ../ownership-error.patch
  patch -Np1 -i ../parallel.patch
  patch -Np1 -i ../perfetto.patch
  patch -Np1 -i ../signin.patch
  patch -Np1 -i ../sizet.patch

  #patch -Np1 -i ../flag-for-search-engine-collection.patch
  #patch -Np1 -i ../flag-to-configure-extension-downloading.patch
  #patch -Np1 -i ../flag-to-disable-beforeunload.patch
  #patch -Np1 -i ../flag-to-enable-potentially-annoying-security-features.patch
  #patch -Np1 -i ../flag-to-force-punycode-hostnames.patch
  #patch -Np1 -i ../flag-to-hide-crashed-bubble.patch
  #patch -Np1 -i ../flag-to-show-avatar-button.patch
  #patch -Np1 -i ../flag-to-stack-tabs.patch

  #patch -Np1 -i ../ipv6-probing-option.patch
  #patch -Np1 -i ../suggestions-url-field.patch
  #patch -Np1 -i ../third-party-ungoogled.patch
  #patch -Np1 -i ../block-trk-and-subdomains.patch
  #patch -Np1 -i ../clear-http-auth-cache-menu-item.patch
  #patch -Np1 -i ../default-to-https-scheme.patch
  patch -Np1 -i ../disable-crash-reporter.patch
  patch -Np1 -i ../disable-domain-reliability.patch
  patch -Np1 -i ../disable-download-quarantine.patch
  patch -Np1 -i ../disable-fonts-googleapis-references.patch
  patch -Np1 -i ../disable-formatting-in-omnibox.patch
  patch -Np1 -i ../disable-gaia.patch
  patch -Np1 -i ../disable-gcm.patch
  #patch -Np1 -i ../disable-google-host-detection.patch
  patch -Np1 -i ../disable-intranet-redirect-detector.patch
  patch -Np1 -i ../disable-mei-preload.patch
  patch -Np1 -i ../disable-network-time-tracker.patch
  patch -Np1 -i ../disable-profile-avatar-downloading.patch
  #patch -Np1 -i ../disable-signin.patch
  patch -Np1 -i ../disable-translate.patch
  patch -Np1 -i ../disable-untraceable-urls.patch
  patch -Np1 -i ../disable-webgl-renderer-info.patch
  patch -Np1 -i ../disable-webrtc-log-uploader.patch
  patch -Np1 -i ../disable-webstore-urls.patch
  #patch -Np1 -i ../enable-page-saving-on-more-pages.patch
  #patch -Np1 -i ../fix-building-without-mdns-and-service-discovery.patch
  #patch -Np1 -i ../fix-building-without-one-click-signin.patch
  #patch -Np1 -i ../fix-building-without-safebrowsing.patch
  #patch -Np1 -i ../fix-learn-doubleclick-hsts.patch
  #patch -Np1 -i ../no-such-option-no-sysroot.patch
  #patch -Np1 -i ../popups-to-tabs.patch
  patch -Np1 -i ../remove-disable-setuid-sandbox-as-bad-flag.patch
  patch -Np1 -i ../remove-fcomplete-member-pointers-cflag.patch
  patch -Np1 -i ../remove-third-party-analytics.patch
  #patch -Np1 -i ../replace-google-search-engine-with-nosearch.patch
  #patch -Np1 -i ../searx.patch

  patch -Np1 -i ../use-local-devtools-files.patch
  patch -Np1 -i ../fix-libva1-compatibility.patch
  patch -Np1 -i ../fix-nullptr-t-namespace.patch

  #patch -Np1 -i ../swiftshader.patch
  #patch -Np1 -i ../third-party-cookies.patch
  patch -Np1 -i ../unrar.patch

  #patch -Np1 -i ../installer.patch
  #patch -Np1 -i ../openh264.patch

  patch -Np1 -i ../default-allocator.patch
  patch -Np1 -i ../define__libc_malloc.patch

##########

patch -Np1 < ../alignof.patch
patch -Np1 < ../ambiguous-overloads.patch
patch -Np1 < ../as-needed.patch
patch -Np1 < ../autocompletematch.patch
patch -Np1 < ../chromedriver-revision.patch
patch -Np1 < ../constexpr.patch
patch -Np1 < ../constructor.patch
patch -Np1 < ../namespace.patch
#patch -Np1 < ../optimize.patch
#patch -Np1 < ../ownership-error.patch
#patch -Np1 < ../polymer.patch
#patch -Np1 < ../ps-print.patch
#patch -Np1 < ../sizet.patch
#patch -Np1 < ../icu.patch
patch -Np1 < ../fontconfig.patch
#patch -Np1 < ../atk.patch
patch -Np1 < ../convertutf.patch
#patch -Np1 < ../event.patch
#patch -Np1 < ../fuzzers.patch
#patch -Np1 < ../parallel.patch
#patch -Np1 < ../bootstrap.patch
#patch -Np1 < ../libcxx.patch
patch -Np1 < ../sysroot.patch

###########

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

  export CC='/bin/ccache /bin/clang'
  export CXX='/bin/ccache /bin/clang++'
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
    'gtk_version=2'
    'remove_webcore_debug_symbols=true'
    'use_gold=false'
    'use_kerberos=false'
    'is_debug=false'
    'enable_vr=false'
    'enable_vulkan=false'
    'is_desktop_linux=true'
    'use_dbus=true'
    'use_system_zlib=true'
    'use_gio=true'
    'use_alsa=true'
    'use_aura=true'
    'use_glib=true'
    'rtc_enable_protobuf=false'
    'use_libpci=false'
    'enable_remoting=false'
    'enable_wayland_server=false'
    'use_gnome_keyring=false'
    'use_sysroot=false'
    'linux_use_bundled_binutils=false'
    'use_custom_libcxx=false'
    'enable_hangout_services_extension=true'
    'enable_widevine=true'
    'enable_nacl=false'
    'enable_swiftshader=false'
    'use_allocator="none"'
    'fatal_linker_warnings=false'
    'target_os="linux"'
    'current_os="linux"'
    'optimize_webui=false'
    'enable_mdns=true'
    'is_cfi=false'
    'use_lld=false'
    'use_thin_lto=false'
    'is_clang=true'
    'clang_use_chrome_plugins=false'
    "google_api_key=\"${_google_api_key}\""
    "google_default_client_id=\"${_google_default_client_id}\""
    "google_default_client_secret=\"${_google_default_client_secret}\""
    'use_vaapi=true'
  )
  # 'use_jumbo_build=true' 'jumbo_file_merge_limit=40' 'is_cfi=false' 'use_lld=false' 'use_thin_lto=false'
  # 'is_clang=true' 'clang_use_chrome_plugins=false'
  # 'use_system_harfbuzz=false' 'use_system_libjpeg=false'
  #     'is_cfi=false'
   # 'use_lld=false'
   # 'use_thin_lto=false'
   # 'is_clang=true'
  # Facilitate deterministic builds (taken from build/config/compiler/BUILD.gn)
  CFLAGS+='   -Wno-builtin-macro-redefined'
  CXXFLAGS+=' -Wno-builtin-macro-redefined'
  CPPFLAGS+=' -D__DATE__=  -D__TIME__=  -D__TIMESTAMP__='

  _flags+=('symbol_level=0')

  # Mimic exclude_unwind_tables=true
  CFLAGS+='   -fno-unwind-tables -fno-asynchronous-unwind-tables'
  CXXFLAGS+=' -fno-unwind-tables -fno-asynchronous-unwind-tables'
  CPPFLAGS+=' -DNO_UNWIND_TABLES'

  ./build/linux/unbundle/replace_gn_files.py --system-libraries "${!_system_libs[@]}"

  #./third_party/libaddressinput/chromium/tools/update-strings.py

  gn gen out/Release --args="${_flags[*]}" --script-executable=/usr/bin/python2

  noti ninja -j7 -C out/Release chrome chrome_sandbox chromedriver

  # ionice -c3 nice -n20 
}

package() {
  cd "$srcdir/chromium-$pkgver"

  install -D out/Release/chrome "$pkgdir/usr/lib/chromium/chromium"
  install -Dm4755 out/Release/chrome_sandbox "$pkgdir/usr/lib/chromium/chrome-sandbox"

  cp \
    out/Release/{chrome_{100,200}_percent,resources}.pak \
    out/Release/{*.bin,chromedriver} \
    "$pkgdir/usr/lib/chromium/"
  install -Dm644 -t "$pkgdir/usr/lib/chromium/locales" out/Release/locales/*.pak

  if [[ -z ${_system_libs[icu]+set} ]]; then
    cp out/Release/icudtl.dat "$pkgdir/usr/lib/chromium/"
  fi

  mv $pkgdir/usr/lib/chromium/chromium $pkgdir/usr/lib/chromium/chrome
}

# vim:set ts=2 sw=2 et:
