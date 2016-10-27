# Maintainer: Michael DeGuzis <mdeguzis@gmail.com>

pkgname=ges-git
pkgver=5.0.99
pkgrel=1
pkgdesc="Multiplayer Only FPS. A recreation of GoldenEye64 as a Half-Life 2 mod using Source Engine SDK 2013."
arch=('i686')
url="https://www.geshl2.com/"
license=('GPLv3')
makedepends=('cmake' 'curl' 'boost-libs' 'boost' 'git' 'gcc' 'glibc' 'libstdc++5')
source=('ges-git::git+https://github.com/goldeneye-source/ges-code.git'
	'python::git+https://github.com/python-cmake-buildsystem/python-cmake-buildsystem.git')
sha256sums=('SKIP'
	    'SKIP')
provides=('ges-git')
conflicts=('ges')

pkgver() {

  cd "${pkgname}"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"

}
prepare()
{

  # Possible conflicts with system libs?
  #echo "set(ENABLE_ZLIB OFF CACHE INTERNAL \"\")" >> "$srcdir/$pkgname/cmake/ges_python.cmake"
  #echo "set(ENABLE_SQLITE3 OFF CACHE INTERNAL \"\")" >> "$srcdir/$pkgname/cmake/ges_python.cmake"
  echo "set(ENABLE_GDBM OFF CACHE INTERNAL \"\")" >> "$srcdir/$pkgname/cmake/ges_python.cmake"

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

  # TEMP ONLY: output ges_python.cmake for build log
  # Sent to pastebin to review later since we are building from a chroot
  cat "$srcdir/$pkgname/cmake/ges_python.cmake" | curl -F 'sprunge=<-' http://sprunge.us
  cat "$srcdir/$pkgname/build/CMakeCache.txt" | curl -F 'sprunge=<-' http://sprunge.us
  cat "$srcdir/$pkgname/build/python/src/python-build/CMakeCache.txt" | curl -F 'sprunge=<-' http://sprunge.us

  cd "${pkgname}/build"

  cmake -DCMAKE_INSTALL_PREFIX=${HOME}/.local/share/Steam/steamapps/sourcemods/gesource	..
  make VERBOSE=1
  make DESTDIR="${pkgdir}" install

}

package()
{

  # TODO
  cd "${srcdir}/${pkgname}/build"
  install -m 755 client.so "${pkgdir}"/usr/bin/
  install -m 755 server.so "${pkgdir}"/usr/bin/

}
