# Maintainer (patched version): @thodnev <at> Telegram
# Original maintainer (of oclint AUR): Moritz Lipp <mlq@pwmt.org>
# 
# This PKGBUILD is a fixed version of one (a bit outdated now)
#    provided by Moritz Lipp

pkgname=oclint
pkgver=0.13.1
pkgrel=2
pkgdesc="A static source code analysis tool to improve quality and reduce
defects for C, C++ and Objective-C"
arch=('i686' 'x86_64')
url="http://oclint.org/"
license=('BSD')
dependencies=('clang' 'clang-analyzer' 'llvm' 'llvm-libs' 'openssl')
makedepends=('clang' 'cmake' 'ninja' 'subversion' 'python' 'llvm' 'libxml2'
'countly-cpp')
source=("https://github.com/oclint/oclint/archive/v${pkgver}.tar.gz"
        "https://raw.githubusercontent.com/thodnev/oclint-aur/master/make_compatible.patch")
sha1sums=('0cc3efb990b058ac73dd695001dce031d4f35750'
	  'e4902f9695f037238b5c2c35c9d6dc1d0fb07d0f')

prepare() {
  cd "$srcdir/$pkgname-$pkgver"
  patch -p2 < $srcdir/make_compatible.patch
}

build() {
  cd "$srcdir/$pkgname-$pkgver/oclint-scripts"
  ./build -llvm-root=/usr -no-analytics -release -j `awk '/^processor/{n+=2}END{print n}' /proc/cpuinfo`
}

package() {
	cd "$srcdir/$pkgname-$pkgver"

  # FIX: Copy llvm LICENSE file for bundle script
  mkdir -p llvm
  cp /usr/include/llvm/Support/LICENSE.TXT llvm

  # Run bundle scripts
  cd oclint-scripts
  ./bundle -llvm-root=/usr -release
  cd ..

  mkdir -p $pkgdir/usr/bin
  cp ./build/oclint-release/bin/oclint-$pkgver $pkgdir/usr/bin/oclint

  mkdir -p $pkgdir/usr/lib/oclint
  cp -r ./build/oclint-release/lib/oclint/* $pkgdir/usr/lib/oclint/
}
