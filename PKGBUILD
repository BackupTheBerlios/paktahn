# Contributor: Leslie P. Polzer <polzer@gnu.org>
# Maintainer: Leslie P. Polzer <polzer@gnu.org>

pkgname=paktahn
pkgver=0.8.2
libver=0.8
pkgrel=1
pkgdesc="The kick-ass package manager for Arch Linux!"
arch=('i686' 'x86_64')
depends=('pacman' 'readline' 'sudo' 'svn')
if [ `arch` == "x86_64" ]; then
    makedepends=('sbcl-with-debugger')
else
    makedepends=('sbcl>=1.0.32')
fi
license=('GPL')
url="http://blog.viridian-project.de/2009/09/19/announcement-paktahn-a-successor-to-yaourt/"
options=(!strip)
install="paktahn.install"
source=("http://viridian-project.de/~sky/paktahn/paktahn-makedeps-$libver.tbz2"
#        "http://viridian-project.de/~sky/paktahn/paktahn-${pkgver}.tar.bz2")
        "paktahn-${pkgver}.tar.bz2")

build() {
  cd "$srcdir"

  # compile
  SBCL=sbcl

  $SBCL \
       --noinform --lose-on-corruption --end-runtime-options \
       --no-userinit --no-sysinit \
       --eval "(pushnew :paktahn-deploy *features*)" \
       --eval "(require :asdf)" \
       --eval "(setf asdf:*central-registry*
                    '(\"$srcdir/\" \"$srcdir/clbuild.paktahn/systems/\"))" \
       --eval "(asdf:oos 'asdf:load-op 'paktahn)" \
       --eval "(pak::build-core :forkp nil)"

  # install binary
  mkdir -p $pkgdir/usr/bin
  install -m755 paktahn $pkgdir/usr/bin

  # install helper scripts
  mkdir -p $pkgdir/usr/lib/paktahn
  for helper in pkgbuild-helper.sh makepkg-helper.sh; do
    install -m755 "$helper" $pkgdir/usr/lib/paktahn
  done

  # create convenience symlink
  cd $pkgdir/usr/bin
  ln -s paktahn pak
}
md5sums=('1c1336880231e88a4e90b32222272ed1'
	 '6e41c15e0f95d2a5257e253b68963ca3')
sha1sums=('643e60fe54c56038e03823280e6d9173ad273f00'
	  '48b72cfef815fded1831c46af5fa9f8d8c012ad1')
