# Maintainer: Léo Flaventin Hauchecorne <hl037.prog@gmail.com>
pkgname=clcbrowser-git
pkgver=1.1
pkgrel=2
pkgdesc="Fully Command line and remote controllable web browser"
arch=('i686' 'x86_64')
url="https://gitorious.org/clcbrowser/clcbrowser"
license=('GPL3')
groups=()
depends=('qt5-base' 'qt5-tools' 'qt5-webkit')
makedepends=('git')
provides=()
conflicts=()
replaces=()
backup=()
options=()
install=
source=()
noextract=()
md5sums=() #generate with 'makepkg -g'
_gitroot=git://gitorious.org/clcbrowser/clcbrowser.git
_gitname=clcbrowser


build() {
   cd "$srcdir"
   msg "Connecting to GIT server...."

   if [[ -d "$_gitname" ]]; then
      cd "$_gitname" && git pull origin
      msg "The local files are updated."
   else
      git clone "$_gitroot" "$_gitname"
   fi

   msg "GIT checkout done or server timeout"
   msg "Starting build..."

   cd "$srcdir"
   rm -rf "$_gitname-build"
   git clone "$_gitname" "$_gitname-build"
   
   
   #
   # Submodules
   #
   
   cd "$_gitname-build"
   git submodule update --init
   cd ..
   
   #
   # BUILD HERE
   #
   
   if test "$CARCH" == i686 ;
      then
      SPEC="linux-g++-32"
   else
      if test "$CARCH" == x86_64
      then
         SPEC="linux-g++-64"
      else
         echo "unknown architecture"
         exit 1
      fi
   fi
   
   

   BUILD="$srcdir/build"
   rm -rf "$BUILD"
   mkdir "$BUILD"
   cd "$BUILD"
   lrelease-qt5 "$srcdir/$_gitname-build/clcbrowser.pro"
   qmake-qt5 "$srcdir/$_gitname-build/clcbrowser.pro" -r -spec $SPEC
   make -w
}

package() {
   BUILD="$srcdir/build"
   cd "$BUILD"
   install -d "$pkgdir/usr/bin" || return 1
   install -Dm755 clcbrowser "$pkgdir/usr/bin" || return 1
   cd "$srcdir/$_gitname-build"
   install -d "$pkgdir/usr/share/icons" || return 1
   install -Dm755 clcbrowser.png "$pkgdir/usr/share/icons/clcbrowser.png" || return 1
   cd linux
   install -d "$pkgdir/usr/share/applications" || return 1
   install -Dm755 clcbrowser.desktop "$pkgdir/usr/share/applications" || return 1
}

# vim:set ts=2 sw=2 et:

