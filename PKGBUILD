# Maintainer : bartus <arch-user-repoᘓbartus.33mail.com>
# shellcheck disable=SC2034,SC2164,SC2154

pkgname=openboard-git
pkgver=v1.6.0a3.r0.gb9af4aa8
pkgrel=1
pkgdesc="Interactive whiteboard software for schools and universities"
arch=('x86_64')
url="http://openboard.ch/index.en.html"
license=('GPL3')
depends=('qt5-base' 'qt5-multimedia' 'qt5-svg' 'qt5-script' 'qt5-webkit' 'qt5-tools' 'qt5-xmlpatterns' 'libpaper' 'bzip2' 'openssl' 'libfdk-aac' 'sdl' 'ffmpeg')
depends+=(quazip)  #drop internal quazip and use system one.
depends+=(poppler) #replace xpdf lib with poppler, simplify the package and remove internal dep.
makedepends=(git)
source=("git://github.com/OpenBoard-org/OpenBoard.git#tag=v1.6.0a3"
        qchar.patch
        openboard.desktop
)
source+=(quazip.diff)
source+=(drop_ThirdParty_repo.patch)
source+=(30fps.patch)
sha256sums=('SKIP'
            'b40fdab85f5921d0404c07db64628a2428a87d39193d2797bbef2e69b1d51549'
            '00688af02006bddeab797f624e5cbae66a5c02f4e14315d87d3f198f74797c17'
            '477e027a6257b12b8a9c9ff6e3ea8814a55b91f5cf58a59ff1497689c126256b'
            'fcc38d87c10d859c30928bcf6c2761f1a5fc81f0d2888f89a2f48280d54fadfc'
            '205062adbbd48d6622341e316e14a5496f73696385a3ed5cda7a89d3e7d2861d')

pkgver() {
  cd "$srcdir/OpenBoard"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "$srcdir/OpenBoard"
  git apply -v "$srcdir"/*{diff,patch}
}

build() {
  cd "$srcdir/OpenBoard"
  qmake OpenBoard.pro -spec linux-g++
  make
}

package() {
  cd "$srcdir/OpenBoard"

  mkdir -p "$pkgdir/opt/openboard"

  for i in customizations etc i18n library; do
    cp -rp "$srcdir/OpenBoard/resources/$i" "$pkgdir/opt/openboard"
  done 

  cp -rp "$srcdir/OpenBoard/resources/images/OpenBoard.png" "$pkgdir/opt/openboard/"
  cp -rp "build/linux/release/product/OpenBoard" "$pkgdir/opt/openboard/"

  install -D -m 644 "$srcdir/openboard.desktop" "$pkgdir/usr/share/applications/openboard.desktop"
  install -d -m 755 "$pkgdir/usr/bin"
  ln -s /opt/openboard/OpenBoard "$pkgdir/usr/bin/openboard"
}
