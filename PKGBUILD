# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://projects.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/lirc
# 						Maintainer: Lukas Fleischer <lfleischer@archlinux.org>
# 						Contributor: Paul Mattal <paul@archlinux.org>

pkgname=lirc
_pkgver=0.9.4d
[[ $_pkgver =~ [a-z]$ ]] && pkgver="${_pkgver:0:-1}.${_pkgver: -1}" || pkgver="$_pkgver"
pkgrel=2
epoch=1
pkgdesc="Linux Infrared Remote Control utilities"
arch=('i686' 'x86_64')
url="http://www.lirc.org/"
license=('GPL')
depends=('alsa-lib' 'libx11' 'libftdi' 'libusb-compat')
makedepends=('help2man' 'alsa-lib' 'libx11' 'libxslt' 'python')
optdepends=('python: for lirc-setup, irdb-get and pronto2lirc'
			'lirc-s6rcserv: lirc s6-rc service'
			'lircd-runitserv: lirc runit service'
			'lircmd-runitserv: lircmd runit service')
provides=('lirc-utils')
conflicts=('lirc-utils')
replaces=('lirc-utils')
backup=('etc/lirc/lirc_options.conf' 'etc/lirc/lircd.conf' 'etc/lirc/lircmd.conf')
source=("https://sourceforge.net/projects/${pkgname}/files/LIRC/${_pkgver}/${pkgname}-${_pkgver}.tar.gz"
        lirc.logrotate
        lirc.tmpfiles)
md5sums=('7763b63db3e203eaec5fb8abe40e7a1e'
         '5c155b063268f1bc48ff0d5e01f53655'
         'febf25c154a7d36f01159e84f26c2d9a')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal



build() {
  cd "${srcdir}/lirc-${_pkgver}"

  HAVE_UINPUT=1 ./configure \
				--prefix=/usr \
				--sbindir=/usr/bin \
				--sysconfdir=/etc \
				--localstatedir=/var \
				
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

  make
}

package() {
  cd "${srcdir}/lirc-${_pkgver}"

  make DESTDIR="${pkgdir}" -j1 install
  
  install -Dm644 "${srcdir}"/lirc.tmpfiles "${pkgdir}"/usr/lib/tmpfiles.d/lirc.conf
  install -Dm644 "${srcdir}"/lirc.logrotate "${pkgdir}"/etc/logrotate.d/lirc

  rmdir "${pkgdir}"/var/{run/lirc/,run/}
}
