# Maintainer: Casey Link < unnamedrambler gmail DOT com>
pkgname=qtilities-git
pkgver=20120121
pkgrel=1
pkgdesc="Building blocks for Qt applications (development version, with docs)"
arch=('i686' 'x86_64')
url="http://www.qtilities.org"
license=('GPL v3')
depends=('qt')
makedepends=('git' 'doxygen' 'graphviz')
source=()
md5sums=()
conflicts=('qtilities' 'qtilities-doc')
_gitroot="git://github.com/JPNaude/Qtilities.git"
_gitname="Qtilities"


build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot $_gitname
    cd $_gitname
  fi

  cd src

  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  # BUILD

  qmake "Qtilities.pro"
  make || return 1

  cd ../doc
  doxygen doxyfile_website
}

package() {
  install -dm755 $pkgdir/usr/lib
  install -dm755 $pkgdir/usr/include
  install -dm755 $pkgdir/usr/share/doc/$pkgname
  cd $srcdir/$_gitname
  cp -rdp ./bin/* $pkgdir/usr/lib/
  cp -r ./doc/output/html_website/* $pkgdir/usr/share/doc/$pkgname/
  cp -r ./include/* $pkgdir/usr/include/

  # now we overwrite the .h headers 
  MODULES=(Core CoreGui ExtensionSystem Logging ProjectManagement Testing)
  for module in $MODULES; do
    cp -f src/$module/source/*.h $pkgdir/usr/include/Qtilities$module/
  done
}

# vim:set ts=2 sw=2 et:
