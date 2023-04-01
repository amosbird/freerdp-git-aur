# Maintainer: Amos Bird <amosbird@gmail.com>

pkgname=freerdp-git
pkgdesc='Free RDP client - git checkout'
pkgver=99999.2.0.0.r1158.gf57449749
pkgrel=1
pkgdesc="Free implementation of the Remote Desktop Protocol (RDP) - git checkout"
arch=(x86_64)
url="https://www.freerdp.com/"
license=(Apache)
depends=(
  dbus-glib
  glibc
  gstreamer
  gst-plugins-base-libs
  libcups
  libgssglue
  libx11
  libxcursor
  libxext
  libxdamage
  libxfixes
  libxkbcommon
  libxi
  libxinerama
  libxkbfile
  libxrandr
  libxrender
  libxtst
  pcsclite
  wayland
)
makedepends=(
  alsa-lib
  cmake
  docbook-xsl
  ffmpeg
  icu
  krb5
  libjpeg-turbo
  libpulse
  libusb
  openssl
  pam
  systemd
  xmlto
  xorgproto
)
provides=(
  libfreerdp2.so
  libfreerdp-client2.so
  libfreerdp-server2
  libfreerdp-shadow2.so
  libfreerdp-shadow-subsystem2.so
  libwinpr2.so
  libwinpr-tools2.so
  libuwac0.so
)
conflicts=('freerdp')
source=('freerdp::git+https://github.com/amosbird/FreeRDP.git')
sha256sums=('SKIP')

pkgver() {
  cd freerdp/

  if GITTAG="$(git describe --abbrev=0 --tags 2>/dev/null)"; then
    printf '99999.%s.r%s.g%s' \
      "$(sed -e "s/^${pkgname%%-git}//" -e 's/^[-_/a-zA-Z]\+//' -e 's/[-_+]/./g' <<< ${GITTAG})" \
      "$(git rev-list --count ${GITTAG}..)" \
      "$(git rev-parse --short HEAD)"
  else
    printf '99999.0.r%s.g%s' \
      "$(git rev-list --count master)" \
      "$(git rev-parse --short HEAD)"
  fi
}

build() {
  cd freerdp/
  cmake -DCMAKE_INSTALL_PREFIX='/usr' \
        -DCMAKE_INSTALL_LIBDIR='lib' \
        -DCMAKE_BUILD_TYPE='None' \
        -DPROXY_PLUGINDIR='/usr/lib/freerdp2/server/proxy/plugins' \
        -DWITH_DSP_FFMPEG=ON \
        -DWITH_FFMPEG=ON \
        -DWITH_PULSE=ON \
        -DWITH_CUPS=ON \
        -DWITH_PCSC=ON \
        -DWITH_JPEG=ON \
        -DWITH_SERVER=ON \
        -DWITH_SWSCALE=ON \
        -DWITH_CHANNELS=ON \
        -DWITH_CLIENT_CHANNELS=ON \
        -DWITH_SERVER_CHANNELS=ON \
        -DCHANNEL_URBDRC_CLIENT=ON \
        -DWITH_VAAPI=ON \
        -DWITH_FUSE=ON \
        -DWITH_ICU=ON \
        -Wno-dev \
        -B build \
        -S .
  make -C build
}

check() {
  cd freerdp/
  ctest --test-dir build --output-on-failure
}

package() {
  depends+=(
    alsa-lib libasound.so
    ffmpeg libavcodec.so libavutil.so libswresample.so libswscale.so
    icu libicuuc.so
    libjpeg-turbo libjpeg.so
    libpulse libpulse.so
    libusb libusb-1.0.so
    openssl libcrypto.so libssl.so
    pam libpam.so
    systemd-libs libsystemd.so
  )

  cd freerdp/
  make DESTDIR="${pkgdir}" install -C build
  install -vDm 644 $_name-$pkgver/{ChangeLog,README.md} -t "$pkgdir/usr/share/doc/$pkgname/"
}
