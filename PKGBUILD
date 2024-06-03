# Maintainer: 7Ji <pugokushin@gmail.com>
# Contributor: Thomas Bächler <thomas@archlinux.org>
# Contributor: Kevin Mihelich <kevin@archlinuxarm.org>
#  - ALARM: Kevin Mihelich: 
#      disable distcc - configure checks for clang first

_pkgname=v4l-utils
pkgname=${_pkgname}-mpp
pkgver=1.26.1
pkgrel=1
pkgdesc="Userspace tools and conversion library for Video 4 Linux, with Rockchip MPP support"
arch=('x86_64' 'aarch64' 'armv7h')
url="https://linuxtv.org/"
provides=("libv4l=${pkgver}" "${_pkgname}=${pkgver}")
replaces=('libv4l')
conflicts=('libv4l' "${_pkgname}")
backup=(etc/rc_maps.cfg)
license=('LGPL')
options=('!distcc')
depends=('hicolor-icon-theme' 'gcc-libs' 'libjpeg-turbo'  'systemd-libs' 'json-c')
makedepends=('qt5-base' 'alsa-lib' 'meson' 'clang' 'doxygen' 'libbpf')
optdepends=('qt5-base: for qv4l2 and qvidcap' 'alsa-lib: for qv4l2')
source=(https://linuxtv.org/downloads/v4l-utils/${_pkgname}-${pkgver}.tar.xz{,.asc})
_patches=(
  '0001-libv4l2-Support-mmap-to-libv4l-plugin.patch'
  '0002-libv4l-mplane-Filter-out-multiplane-formats.patch'
  '0003-libv4l-Disallow-conversion-by-default.patch'
)
_patch_commit='94a4679911aeb3e90f2e9ac955e83bde9ae0d8b5'
_patch_parent="https://github.com/JeffyCN/meta-rockchip/raw/${_patch_commit}/recipes-multimedia/v4l2apps/v4l-utils/"
for _patch in ${_patches[@]}; do
  source+=("${_patch_parent}${_patch}")
done
sha256sums=(
  '4a71608c0ef7df2931176989e6d32b445c0bdc1030a2376d929c8ca6e550ec4e'
  'SKIP'
  '2b6b3e39a0bc46518bb1c77b165412818c8d3be2f0d1ba63aff762243e7860e3'
  '06f4516e03e4f387732e6b357e1d44a3546ed56b82f5d93fb090f30fcccc599b'
  'e594ff7c90ad7a1ac9295178349ccf6b51050696d1e79f8ea632e2a73360eb61'
)
validpgpkeys=('05D0169C26E41593418129DF199A64FADFB500FF') # Gregor Jasny <gjasny@googlemail.com>

_srcname="${_pkgname}-${pkgver}"

prepare() {
  cat ${_patches[@]} | patch -p1 -d "${_srcname}"
  # HACK: inform upstream to make this configurable
  cd "${_srcname}"
  sed -i 's/sbin/bin/' utils/v4l2-dbg/meson.build
}

build() {
  arch-meson -Dgconv=disabled "${_srcname}" build
  meson compile -C build
}

package() {
  meson install -C build --destdir "$pkgdir"
}
