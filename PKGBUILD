# Maintainer: Amos Bird <amosbird@gmail.com>

pkgname=freerdp-git
pkgdesc='Free RDP client - git checkout'
pkgver=2.0.0.rc4.r1096.g94711084a
pkgrel=1
pkgdesc="Free implementation of the Remote Desktop Protocol (RDP)"
arch=('x86_64')
url="http://www.freerdp.com/"
license=('Apache')
depends=('dbus-glib' 'glib2' 'glibc' 'gsm' 'gstreamer' 'gst-plugins-base-libs'
'lame' 'libcups' 'libjpeg-turbo' 'libgssglue' 'libsoxr' 'libusb' 'libxkbcommon'
'libxinerama' 'libxkbfile' 'libxrandr' 'mbedtls' 'openssl' 'pam' 'pcsclite'
'wayland')
makedepends=('alsa-lib' 'cmake' 'docbook-xsl' 'faac' 'faad2' 'ffmpeg' 'krb5'
'libpulse' 'systemd-libs' 'xmlto' 'xorgproto')
provides=('freerdp' 'libwinpr-tools2.so' 'libfreerdp-client2.so' 'libfreerdp2.so'
'libwinpr2.so')
conflicts=('freerdp')
source=('freerdp::git://github.com/amosbird/FreeRDP.git')
sha256sums=('SKIP')

pkgver() {
  cd freerdp/

  if GITTAG="$(git describe --abbrev=0 --tags 2>/dev/null)"; then
    printf '%s.r%s.g%s' \
      "$(sed -e "s/^${pkgname%%-git}//" -e 's/^[-_/a-zA-Z]\+//' -e 's/[-_+]/./g' <<< ${GITTAG})" \
      "$(git rev-list --count ${GITTAG}..)" \
      "$(git rev-parse --short HEAD)"
  else
    printf '0.r%s.g%s' \
      "$(git rev-list --count master)" \
      "$(git rev-parse --short HEAD)"
  fi
}

build() {
  cd freerdp/
  cmake -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_INSTALL_LIBDIR=lib \
        -DWITH_MBEDTLS=ON \
        -DWITH_PULSE=ON \
        -DWITH_CUPS=ON \
        -DWITH_PCSC=ON \
        -DWITH_JPEG=ON \
        -DWITH_GSM=ON \
        -DWITH_LAME=ON \
        -DWITH_FAAD2=ON \
        -DWITH_FAAC=ON \
        -DWITH_SOXR=ON \
        -DWITH_SERVER=ON \
        -DWITH_CHANNELS=ON \
        -DWITH_CLIENT_CHANNELS=ON \
        -DWITH_SERVER_CHANNELS=ON \
        -DCHANNEL_URBDRC_CLIENT=ON \
        -B build \
        -S .
  make VERBOSE=1 -C build
}

package() {
  depends+=('libasound.so' 'libavcodec.so' 'libavutil.so' 'libfaac.so'
  'libfaad.so' 'libpulse.so' 'libsystemd.so' 'libudev.so')
  cd freerdp/
  make DESTDIR="${pkgdir}" install -C build
}
