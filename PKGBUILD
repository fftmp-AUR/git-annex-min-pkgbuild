# There are 2 most popular tools for build haskell projects: cabal and stack.
# Their entry points is Setup.hs and stack.yaml correspondingly.

# from  https://wiki.archlinux.org/title/Haskell:
# GHC does not provide stable ABI, so usage of shared libraries is difficult.
# archlinux package maintainers decide to support dynamic versions for haskell packages and update/recompile it every time, when GHC changes.

# AUR packages seems prefer static bilding - compile all dependencies inside result binary to avoid of troubles with ABI.
# This package use dynamic way (for ability to reuse libraries), so:
# 1. There are a lot of dependencies.
# 2. Possibly need to rebuild package after GHC changes.

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
         'haskell-quickcheck' 'haskell-random' 'haskell-regex-tdfa'
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
    uusi -d persistent-template git-annex.cabal
    sed -i 's/MIN_VERSION_persistent_template/MIN_VERSION_persistent/' Database/ContentIdentifier.hs Database/Export.hs Database/Fsck.hs Database/Keys/SQL.hs
}
build() {
  cd git-annex
  # build flags from git-annex/BuildFlags.hs
  _opt_flags=(
	-Assistant
	-Webapp
	-Pairing #https://git-annex.branchable.com/assistant/local_pairing_walkthrough/
	-S3
	-WebDAV
	-FsEvents # MacOS analog of inotify
	-DBus
	-DesktopNotify
	-TorrentParser
	-benchmark # present only in git-annex/stack.yaml
  
  )
  runhaskell Setup configure -O --prefix=/usr --enable-executable-dynamic --disable-library-vanilla \
    --docdir="/usr/share/doc/$pkgname" \

    -f-androidsplice -f-android -fproduction \
      -f-benchmark
    make GHC="ghc -dynamic" BUILDER=./Setup BUILDEROPTIONS=$MAKEFLAGS
}

package() {
  cd git-annex
  make GHC="ghc -dynamic" BUILDER=./Setup DESTDIR="$pkgdir" install

  rm "$pkgdir"/usr/share/doc/git-annex/COPYRIGHT
  rmdir "$pkgdir"/usr/share/doc/git-annex "$pkgdir"/usr/share/doc
}
