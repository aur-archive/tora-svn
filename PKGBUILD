# Maintainer: Tom Kuther <gimpel@sonnenkinder.org>
pkgname=tora-svn
_svnname=tora
pkgver=3.0.0.svn.4997      
pkgrel=1
pkgdesc="Toolkit for databases with support for Oracle, MySQL and PostgreSQL"
arch=('i686' 'x86_64')
url="http://tora.sourceforge.net"
license=('GPL')
provides=('tora')
replaces=('tora')
conflicts=('tora')
depends=('qscintilla' 'oracle-instantclient-basic' 'graphviz' 'poppler' 'qt4')
makedepends=('cmake' 'oracle-instantclient-sdk' 'subversion' 'boost')
options=()
install=$pkgname.install
source=('tora::svn+https://svn.code.sf.net/p/tora/code/trunk/tora/'
        'tora.desktop')
md5sums=('SKIP'
         '089754af8d9d5a537d3ef44929f6aa28')

pkgver() {
  cd $_svnname
  _major=`grep 'SET (VERSION_MAJOR' CMakeLists.txt|awk -F'("|")' '{print $2}'`
  _minor=`grep 'SET (VERSION_MINOR' CMakeLists.txt|awk -F'("|")' '{print $2}'`
  _patch=`grep 'SET (VERSION_PATCH' CMakeLists.txt|awk -F'("|")' '{print $2}'`
  _svnrev=`svnversion | tr -d [A-z]`
  echo "${_major}.${_minor}.${_patch}.svn.${_svnrev}"
}

build() {
  cd $_svnname

  mkdir build
  cd build
  cmake .. \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_BUILD_TYPE=Release \
    -DLIB_SUFFIX="" \
    -DBOOST_ROOT=/usr \
    -DBOOST_INCLUDEDIR=/usr/include/boost \
    -DBOOST_LIBRARYDIR=/usr/lib \
    -DENABLE_DB2=0 \
    -DENABLE_TERADATA=0 \
    -DUSE_EXPERIMENTAL=1 \
    -DWANT_INTERNAL_LOKI=ON
  make
}

package() {
  cd $_svnname/build
  make DESTDIR="${pkgdir}" install

  # -DLIB_SUFFIX="" breakage, fu...
  test -d "${pkgdir}"/usr/lib64 && mv "${pkgdir}"/usr/lib64 "${pkgdir}"/usr/lib

  mkdir -p "${pkgdir}/usr/share/applications"
  mkdir -p "${pkgdir}/usr/share/pixmaps"
  install -m644 ../src/icons/tora.xpm "${pkgdir}/usr/share/pixmaps"
  install -m644 "${srcdir}"/tora.desktop "${pkgdir}/usr/share/applications"
}
