# Maintainer: Husam Bilal <me@husam.dev>
# Contributor: henning mueller <henning@orgizm.net>

pkgname=ruby-libvirt
pkgver=0.8.0
pkgrel=1
pkgdesc='Ruby bindings for libvirt.'
arch=(i686 x86_64)
license=(GPL)
url=http://libvirt.org/ruby/
depends=(ruby libvirt)
makedepends=(rubygems ruby-rake ruby-rdoc)
source=($pkgname::git+https://gitlab.com/libvirt/libvirt-ruby.git#tag=$pkgname-$pkgver)
sha256sums=('SKIP')

build() {
  cd "$pkgname"
  rake gem
}

check() {
  cd "$pkgname"
  rake test
}

package() {
  local _gemdir="$(gem env gemdir)"

  cd "$pkgname"
  gem install \
    --local \
    --verbose \
    --ignore-dependencies \
    --no-user-install \
    --install-dir "$pkgdir/$_gemdir" \
    --bindir "$pkgdir/usr/bin" \
    "pkg/$pkgname-$pkgver.gem"

  # remove unrepreducible files
  rm -frv \
    "$pkgdir/$_gemdir/cache/" \
    "$pkgdir/$_gemdir/gems/$pkgname-$pkgver/vendor/" \
    "$pkgdir/$_gemdir/doc/$pkgname-$pkgver/ri/ext/"

  find "$pkgdir/$_gemdir/gems/" \
    -type f \
    \( \
        -iname "*.o" -o \
        -iname "*.c" -o \
        -iname "*.so" -o \
        -iname "*.time" -o \
        -iname "gem.build_complete" -o \
        -iname "Makefile" \
    \) \
    -delete

  find "$pkgdir/$_gemdir/extensions/" \
    -type f \
    \( \
      -iname "mkmf.log" -o \
      -iname "gem_make.out" \
    \) \
    -delete
}
