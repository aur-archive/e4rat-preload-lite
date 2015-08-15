# Maintainer: Thomas Decoster <acid.sploit@gmail.com>
pkgname=e4rat-preload-lite
pkgver=0.1
pkgrel=3
epoch=
pkgdesc="Replacement for e4rat-preload, which takes less time to load the inodes. The savings come from using pure C with no external librairy references and by preloading only the first 100 files (both inodes and file contents) before starting /sbin/init, then continuing to load the remaining files in parallel with the normal boot sequence."
arch=('any')
url="http://e4rat-l.bananarocker.org/"
license=('GPL')
groups=()
depends=('e4rat')
makedepends=()
checkdepends=()
optdepends=()
provides=()
conflicts=()
replaces=()
backup=()
options=()
install=
changelog=
source=($url$pkgname-$pkgver.tar.gz)
noextract=()
md5sums=('c2250f064da1c883f8f58f3af70cc7a8') #generate with 'makepkg -g'

build() {
  cd "$srcdir/$pkgname-$pkgver"
  
  echo " "
  echo "Searching for startup.log"
  e4rat_list=`grep startup_log_file /etc/e4rat.conf | grep -v ";" | grep -v grep | awk '{print $2}'`
  src_list=`grep "#define LIST" e4rat-preload-lite.c | grep -v grep | awk '{print $3}'` 
  echo " -> $e4rat_list"
  if [ $e4rat_list ] && [ -f $e4rat_list ]
  then
    sed -i "s|$src_list|\"$e4rat_list\"|g" e4rat-preload-lite.c
    echo "Patched e4rat-preload-lite.c for $e4rat_list"
  else
    echo 'Using default startup.log location, no need to patch'
  fi
  echo " "

  echo "Searching for init configuration"
  e4rat_init=`grep init /etc/e4rat.conf | grep -v ";" | grep -v grep | awk '{print $2}'`
  src_init=`grep "#define INIT" e4rat-preload-lite.c | grep -v grep | awk '{print $3}'`
  echo " -> $e4rat_init"
  if [ $e4rat_init ] && [ -x $e4rat_init ]
  then
    sed -i "s|$src_init|\"$e4rat_init\"|g" e4rat-preload-lite.c
    echo "Patched e4rat-preload-lite.c for $e4rat_init"
  else
    echo 'Using default /sbin/init, no need to patch'
  fi
  echo " "

  gcc -std=c99 -Wall -O2 -o e4rat-preload-lite e4rat-preload-lite.c
  strip -s e4rat-preload-lite
  echo "Finished compiling e4rat-preload-lite"
}

check() {
  cd "$srcdir/$pkgname-$pkgver"
}

package() {
  cd "$srcdir/$pkgname-$pkgver"
  mkdir -p "$pkgdir/usr/sbin/"
  cp  e4rat-preload-lite "$pkgdir/usr/sbin/"
}

# vim:set ts=2 sw=2 et:
