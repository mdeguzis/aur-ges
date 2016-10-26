# Maintainer: Electric Prism <electricprism@gmail.com>
# Contributor: Electric Prism <electricprism@gmail.com>
# Contributor: Michael DeGuzis <mdeguzis@gmail.com>

pkgname=ges-git
pkgver=5.0.99
pkgrel=1
pkgdesc="Multiplayer Only FPS. A recreation of GoldenEye64 as a Half-Life 2 mod using Source Engine SDK 2013."
arch=('i686')
url="https://www.geshl2.com/"
license=('GPLv3')
makedepends=('cmake' 'boost-libs' 'boost' 'git' 'gcc' 'glibc' 'libstdc++5')
source=('ges-git::git+https://github.com/ElectricPrism/ges-code.git'
				'python::git+https://github.com/python-cmake-buildsystem/python-cmake-buildsystem.git')
sha256sums=('SKIP'
						'SKIP')
provides=('ges-git')
conflicts=('ges')

pkgver() {

  cd "$pkgname"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"

}
prepare()
{

	# Enter Package Source
	cd "${srcdir}/${pkgname}"

	# Init submodules
	git submodule init
	git config submodule.python.url thirdparty/python
	git submodule update

	# Setup build environment
	if [[ -d build ]]; then
		rm -rf build
	fi

  mkdir build

}

build()
{

	cd "${srcdir}/${pkgname}/build"
	cmake -DCMAKE_INSTALL_PREFIX=${HOME}/.local/share/Steam/steamapps/sourcemods/gesource ..
	make -C build
	make DESTDIR="${pkgdir}" install -C build

}

package()
{

	# TODO
	cd "${srcdir}/${pkgname}/build"
	install -m 755 client.so "${pkgdir}"/usr/bin/
	install -m 755 server.so "${pkgdir}"/usr/bin/
}
