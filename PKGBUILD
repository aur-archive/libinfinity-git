# Maintainer: Willow Walthall <ghthor@gmail.com>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
pkgname=libinfinity-git
pkgver=20130330
pkgrel=1
pkgdesc="An implementation of the Infininote protocol written in GObject-based C"
arch=('i686' 'x86_64')
url="http://gobby.0x539.de"
license=('GPL')
depends=('gnutls' 'gsasl' 'gtk-doc' 'glib2' 'gob2' 'libxml++' 'gtk2')
optdepends=('avahi')
makedepends=('git')
#provides=('libinfinity')
#conflicts=('libinfinity')
source=('automake-1.13.patch')
noextract=('automake-1.13.patch')
sha1sums=('6207dbad8ab20f73dabc78da4d7416e28fccf7f4')

_gitroot=git://git.0x539.de/git/infinote.git
_gitname=infinote

build() {
  cd ${srcdir}
  msg "Connecting to git.0x539.de GIT server..."

  if [ -d ${srcdir}/$_gitname ] ; then
    cd $_gitname && git pull origin
    cd ${srcdir}
    msg "The local files are updated."
  else
    git clone $_gitroot $_gitname
  fi

  msg "GIT checkout done or server timeout"

  # remove any existing build folder
  if [ -d ${srcdir}/$_gitname-build ] ; then
    rm -rf ${srcdir}/$_gitname-build
  fi

  # Create a copy to build from without the .git folder
  mkdir ${srcdir}/$_gitname-build
  cd ${srcdir}/$_gitname && ls -A | grep -v .git | xargs -d '\n' cp -r -t ${srcdir}/$_gitname-build
  cd ${srcdir}/$_gitname-build

  # Apply the patch that enables automake-1.12
  msg "Applying patch to autogen.sh that enables automake-1.13"
  git apply ${srcdir}/automake-1.13.patch

  msg "Starting make..."
  ./autogen.sh --prefix=/usr || return 1
  make || return 1
  make DESTDIR="${pkgdir}" install || return 1
}

# vim:set ts=2 sw=2 et:
