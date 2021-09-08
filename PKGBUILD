pkgname=mingw-w64-libmixmod
pkgver=3.2.2
pkgrel=1
pkgdesc="Model-Based Unsupervised and Supervised Classification"
arch=('any')
url="http://www.mixmod.org"
license=('GPL')
depends=('mingw-w64-crt')
makedepends=('mingw-w64-cmake')
options=('!buildflags' '!strip' 'staticlibs')
source=("git+https://github.com/reimen5125/libmixmod.git")
sha1sums=('SKIP')

_architecture="i686-w64-mingw32 x86_64-w64-mingw32"

prepare () {
  cd "$srcdir"/libmixmod
  # install import/export libs
  sed -i "s|LIBRARY DESTINATION lib|LIBRARY DESTINATION lib ARCHIVE DESTINATION lib RUNTIME DESTINATION bin|g" SRC/CMakeLists.txt

  # build static too
  sed -i "s| SHARED | |g" SRC/CMakeLists.txt
}

build () {
  cd "$srcdir"/libmixmod
  for _arch in $_architecture; do
    unset LDFLAGS
    mkdir -p build-${_arch}-static && pushd build-${_arch}-static
    ${_arch}-cmake \
      -DBUILD_SHARED_LIBS=OFF ..
    make
    popd
    mkdir -p build-${_arch} && pushd build-${_arch}
    ${_arch}-cmake ..
    make
    popd
  done
}

package () {
  for _arch in $_architecture; do
    cd "$srcdir"/libmixmod/build-${_arch}-static
    make install DESTDIR="$pkgdir"
    cd "$srcdir"/libmixmod/build-${_arch}
    make install DESTDIR="$pkgdir"
    rm -r "$pkgdir"/usr/${_arch}/share 
    ${_arch}-strip --strip-unneeded "$pkgdir"/usr/${_arch}/bin/*.dll
    ${_arch}-strip -g "$pkgdir"/usr/${_arch}/lib/*.a
  done
}

