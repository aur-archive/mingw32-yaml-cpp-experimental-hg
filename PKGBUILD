# Maintainer: Daniel Kirchner <daniel at ekpyron dot org>
pkgname=mingw32-yaml-cpp-experimental-hg
pkgver=442
pkgrel=4
pkgdesc="YAML parser and emitter in C++, written around the YAML 1.2 spec, experimental API (mingw32)"
license=('MIT')
arch=('i686' 'x86_64')
url="http://code.google.com/p/yaml-cpp/"
depends=('mingw32-runtime' 'mingw32-boost')
makedepends=('cmake' 'mingw32-gcc')
conflicts=('mingw32-yaml-cpp')
source=('yaml-cpp-pkg-config.patch')
md5sums=('fc8d36f00b6c0535b3229d8dea31a888')
_hgroot="https://code.google.com/p"
_hgrepo="yaml-cpp"
options=(!strip !buildflags)

_targetarch=i486-mingw32

build() {
  # mingw32 has problems with --hash-style=gnu (default value)
  unset LDFLAGS

  cd "${srcdir}/$_hgrepo"
  
  msg "Updating to new API..."
  hg up new-api || return 1
  
  msg "Applying patch..."
  patch -Np1 -i ../yaml-cpp-pkg-config.patch
  
  msg "Starting make..."
  
  rm -rf "$srcdir/$_hgrepo-build"
  mkdir "$srcdir/$_hgrepo-build"
  cd "$srcdir/$_hgrepo-build"
  
  echo "SET(CMAKE_SYSTEM_NAME Windows)" > win32.cmake
  echo "SET(CMAKE_C_COMPILER ${_targetarch}-gcc)" >> win32.cmake
  echo "SET(CMAKE_CXX_COMPILER ${_targetarch}-g++)" >> win32.cmake
  echo "SET(CMAKE_RC_COMPILER ${_targetarch}-windres)" >> win32.cmake
  echo "SET(CMAKE_FIND_ROOT_PATH /usr/${_targetarch})" >> win32.cmake
  echo "SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)" >> win32.cmake
  echo "SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)" >> win32.cmake
  echo "SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)" >> win32.cmake
  echo "SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)" >> win32.cmake
  cmake ../$_hgrepo \
    -DCMAKE_TOOLCHAIN_FILE=win32.cmake \
    -DCMAKE_INSTALL_PREFIX="/usr/${_targetarch}"
  make
}

package() {
  cd "${srcdir}/$_hgrepo-build"
  make install DESTDIR="${pkgdir}"
  mv "${pkgdir}/usr/${_targetarch}/bin/pkgconfig" "${pkgdir}/usr/${_targetarch}/lib"
  rm -rf "${pkgdir}/usr/${_targetarch}/bin"
}
