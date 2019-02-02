# $Id$
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Arch Haskell Team <arch-haskell@haskell.org>

pkgname=git-annex
pkgver=7.20190129
pkgrel=1
pkgdesc="Manage files with git, without checking their contents into git"
url="http://git-annex.branchable.com/"
license=("AGPL3")
arch=('x86_64')
depends=('git' 'lsof' 'rsync' 'ghc-libs' 'haskell-aeson' 'haskell-async'
         'haskell-blaze-builder' 'haskell-bloomfilter' 'haskell-byteable' 'haskell-case-insensitive'
         'haskell-concurrent-output' 'haskell-connection' 'haskell-conduit'
         'haskell-crypto-api' 'haskell-cryptonite'
         'haskell-dbus' 'haskell-disk-free-space' 'haskell-dlist'
         'haskell-edit-distance' 'haskell-esqueleto' 'haskell-exceptions' 'haskell-fdo-notify'
         'haskell-feed' 'haskell-hslogger' 'haskell-http-client'
         'haskell-http-client-tls' 'haskell-http-conduit' 'haskell-http-types'
         'haskell-magic' 'haskell-memory' 'haskell-monad-control' 'haskell-monad-logger'
         'haskell-network' 'haskell-network-info'
         'haskell-network-uri' 'haskell-old-locale' 'haskell-optparse-applicative'
         'haskell-path-pieces' 'haskell-persistent' 'haskell-persistent-sqlite'
         'haskell-persistent-template' 'haskell-quickcheck' 'haskell-random' 'haskell-regex-tdfa'
         'haskell-resourcet' 'haskell-safesemaphore' 'haskell-sandi' 'haskell-securemem'
         'haskell-socks' 'haskell-split' 'haskell-stm-chans' 'haskell-tagsoup'
         'haskell-tasty' 'haskell-tasty-hunit' 'haskell-tasty-quickcheck' 'haskell-tasty-rerun'
         'haskell-unordered-containers' 'haskell-utf8-string' 'haskell-uuid' 'haskell-vector'
        )
makedepends=('chrpath' 'ghc' 'haskell-data-default' 'haskell-ifelse' 'haskell-unix-compat')
source=("git+https://git.joeyh.name/git/git-annex.git#tag=$pkgver")
sha512sums=('SKIP')

prepare() {
    cd git-annex
}
build() {
  cd git-annex

  runhaskell Setup configure -O --prefix=/usr --enable-executable-dynamic --disable-library-vanilla \
    --docdir="/usr/share/doc/$pkgname" \
    -fconcurrentoutput -f-torrentparser \
    -f-androidsplice -f-android -fproduction -f-pairing -f-webapp \
    -f-assistant -f-webdav -f-s3 -f-benchmark -fdbus -fmagicmime
  runhaskell Setup build
}

package() {
  cd git-annex
  runhaskell Setup copy --destdir="$pkgdir"
  make GHC="ghc -dynamic" BUILDER=true DESTDIR="$pkgdir" -j1 install-misc

  rm "$pkgdir"/usr/share/doc/git-annex/COPYRIGHT
  rmdir "$pkgdir"/usr/share/doc/git-annex "$pkgdir"/usr/share/doc
}
