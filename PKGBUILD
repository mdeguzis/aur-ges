# Maintainer: Michael DeGuzis <mdeguzis@gmail.com>
# Upstream: https://github.com/goldeneye-source/ges-code.git

# This is a fork to work around linking issues with python/python-boost on Arch
# Linux systems. Currently, this PKGBUILD uses system python/python-boost.
# It is unknown, until further testing, what impact this will have.

# The second line of makedepends was added to satisfy deps needed by
# the original python/python-boost build upstream

pkgname=ges-git
pkgver=5.0.99
pkgrel=1
pkgdesc="Multiplayer Only FPS. A recreation of GoldenEye64 as a Half-Life 2 mod using Source Engine SDK 2013."
arch=('i686')
url="https://www.geshl2.com/"
license=('GPLv3')
makedepends=('cmake' 'curl' 'boost-libs' 'boost' 'git' 'gcc' 'glibc' 'libstdc++5' 
	     'python' 'bzip2' 'expat' 'zlib')
source=('ges-git::git+https://github.com/professorkaos64/ges-code.git'
	'python::git+https://github.com/python-cmake-buildsystem/python-cmake-buildsystem.git')
sha256sums=('SKIP'
	    'SKIP')
provides=('ges-git')
conflicts=('ges')

# disable pkgver until build fixed so git pull doesn't clash with local build tests

#pkgver() {
#
#  cd "${pkgname}"
#  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"

#}

prepare()
{

  # Possible conflicts with system libs?
  #echo "set(ENABLE_ZLIB OFF CACHE INTERNAL \"\")" >> "$srcdir/$pkgname/cmake/ges_python.cmake"
  #echo "set(ENABLE_SQLITE3 OFF CACHE INTERNAL \"\")" >> "$srcdir/$pkgname/cmake/ges_python.cmake"
  #echo "set(ENABLE_GDBM OFF CACHE INTERNAL \"\")" >> "$srcdir/$pkgname/cmake/ges_python.cmake"

  # Enter Package Source
  cd "${pkgname}"

  # Init submodules
  git submodule init thirdparty/python
  git config submodule.python.url ../python
  git submodule update thirdparty/python

  # Setup build environment
  if [[ -d build ]]; then
  	rm -rf build
  fi

  mkdir build

}

build()
{

  cd "${pkgname}/build"

  cmake -DCMAKE_INSTALL_PREFIX=${HOME}/.local/share/Steam/steamapps/sourcemods/gesource	..
  make VERBOSE=1

}

package()
{

  # TODO
  cd "${srcdir}/${pkgname}/build"
  make DESTDIR="${pkgdir}" install
  
  #install -m 755 client.so "${pkgdir}"/usr/bin/
  #install -m 755 server.so "${pkgdir}"/usr/bin/

}
