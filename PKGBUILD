# Maintainer: Thomas Dziedzic <gostrc@gmail.com>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: John Proctor <jproctor@prium.net>
# Contributor: Jeramy Rutley <jrutley@gmail.com>

pkgname=(ruby ruby-docs)
pkgver=2.4.0
pkgrel=1
arch=(i686 x86_64)
url='http://www.ruby-lang.org/en/'
license=(BSD custom)
makedepends=(gdbm openssl libffi doxygen graphviz libyaml ttf-dejavu tk)
options=(!emptydirs)
source=(https://cache.ruby-lang.org/pub/ruby/${pkgver:0:3}/ruby-${pkgver}.tar.xz
        gemrc
        patch_variable)
sha1sums=('038804bbd0e77508dd2510b729a9f3b325489b2e'
          'dc536754c8fac2c3d82965c5a708cd8f79562d98'
          'SKIP')

build() {
  cd ruby-${pkgver}

  patch < ${srcdir}/patch_variable

  PKG_CONFIG=/usr/bin/pkg-config ./configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --sharedstatedir=/var/lib \
    --libexecdir=/usr/lib/ruby \
    --enable-shared \
    --disable-rpath \
    --with-dbm-type=gdbm_compat

  make
}

check() {
  cd ruby-${pkgver}

  make test
}

package_ruby() {
  pkgdesc='An object-oriented language for quick and easy programming'
  depends=(gdbm openssl libffi libyaml gmp zlib)
  optdepends=(
      'ruby-docs: Ruby documentation'
      'tk: for Ruby/TK'
  )
  provides=(rubygems rake)
  conflicts=(rake)
  backup=(etc/gemrc)
  install=ruby.install

  cd ruby-${pkgver}

  make DESTDIR="${pkgdir}" install-nodoc

  install -D -m644 ${srcdir}/gemrc "${pkgdir}/etc/gemrc"

  install -D -m644 COPYING "${pkgdir}/usr/share/licenses/ruby/LICENSE"
  install -D -m644 BSDL "${pkgdir}/usr/share/licenses/ruby/BSDL"
}

package_ruby-docs() {
  pkgdesc='Documentation files for ruby'

  cd ruby-${pkgver}

  make DESTDIR="${pkgdir}" install-doc install-capi

  install -D -m644 COPYING "${pkgdir}/usr/share/licenses/ruby-docs/LICENSE"
  install -D -m644 BSDL "${pkgdir}/usr/share/licenses/ruby-docs/BSDL"
}