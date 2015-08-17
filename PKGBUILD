# $Id$
# Maintainer : Thomas Baechler <thomas@archlinux.org>

pkgname=nvidia-275xx-ck
pkgver=275.43
_extramodules=extramodules-3.3-ck
_kernver="$(cat /lib/modules/${_extramodules}/version)"
pkgrel=8
pkgdesc="NVIDIA 275-xx drivers for linux-ck."
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=('linux-ck<3.4' 'linux-ck>=3.3' "nvidia-utils-275xx=${pkgver}")
makedepends=('linux-ck-headers<3.4' 'linux-ck-headers>=3.3')
conflicts=('nvidia-96xx' 'nvidia-173xx' 'nvidia')
license=('custom')
install=nvidia.install
options=(!strip)

if [ "$CARCH" = "i686" ]; then
    _arch='x86'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}"
    source=("http://us.download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums=('aba5889666208d6b69aa6b27ff608842')
elif [ "$CARCH" = "x86_64" ]; then
    _arch='x86_64'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32"
    source=("http://us.download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums=('5a80b13f0e92e33367d49866f6377dc1')
fi

build() {
    cd "${srcdir}"
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}/kernel"
    # fix for 3.3 kernel
    sed 's/CFLAGS="$CFLAGS -I$SOURCES\/arch\/x86\/include"/CFLAGS="$CFLAGS \
                   -I$SOURCES\/arch\/x86\/include \
                   -I$SOURCES\/arch\/x86\/include\/generated"/' -i conftest.sh
    make SYSSRC=/lib/modules/"${_kernver}/build" module
}

package() {
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
        "${pkgdir}/lib/modules/${_extramodules}/nvidia.ko"
    install -d -m755 "${pkgdir}/etc/modprobe.d"
    echo "blacklist nouveau" >> "${pkgdir}/etc/modprobe.d/nouveau_blacklist.conf"
    sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia.install"
    gzip "${pkgdir}/lib/modules/${_extramodules}/nvidia.ko"
}
