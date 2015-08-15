# $Id$
# Maintainer: Faruk Diblen 
pkgname=g77
pkgver=3.4.6
pkgrel=2
pkgdesc="The GNU FORTRAN 77 Compiler"
arch=(i686 x86_64)
url="http://gcc.gnu.org/onlinedocs/gcc-3.4.6/g77"
license=('GPL') 
depends=('libstdc++5' 'gcc-libs')
options=('!libtool')
source=(ftp://gcc.gnu.org/pub/gcc/releases/gcc-${pkgver}/gcc-{core,g77}-${pkgver}.tar.bz2 \
	gcc-localeversion.patch gcc-3.3-pure64.patch gcc-i386-info.patch)
md5sums=('5324ace5145b12afd9ca867af7ec084d'
         'eb4c248fa10a96e8d9edc9831c75a895'
         'e93d6f49b254dc2879a4e181603599b0'
         'eb834abd7620a5f11492ee2c243b8346'
         'bf4511ff366f3d92c150b207f94ec269')

build() {

  export CFLAGS="-O2 -pipe"
  export CXXFLAGS="-O2 -pipe"
  export LDFLAGS=""

  cd ${startdir}/src/gcc-${pkgver}

  patch -Np0 -i ${startdir}/src/gcc-localeversion.patch || return 1

  if [ "${CARCH}" = "x86_64" ]; then
    patch -Np1 -i ../gcc-3.3-pure64.patch || return 1
  else
    patch -Np1 -i ../gcc-i386-info.patch || return 1
  fi

  # Don't run fixincludes
  sed -i -e 's@\./fixinc\.sh@-c true@' gcc/Makefile.in
  # Don't install libiberty
  sed -i 's/install_to_$(INSTALL_DEST) //' libiberty/Makefile.in

  mkdir ../gcc-build
  cd ../gcc-build
  ../gcc-${pkgver}/configure --prefix=/usr --enable-shared \
      --enable-languages=f77 --enable-threads=posix \
      --mandir=/usr/share/man --libexecdir=/usr/lib \
      --enable-__cxa_atexit  --disable-multilib --libdir=/usr/lib \
      --enable-clocale=gnu --program-suffix=-3.4 


  make bootstrap || return 1
  make -j1 DESTDIR=${startdir}/pkg install || return 1

  rm -f ${startdir}/pkg/usr/lib/lib{stdc++,supc++,gcc_s}.*
  rm -f ${startdir}/pkg/usr/share/locale/*/LC_MESSAGES/libstdc++.mo
  rm -rf ${startdir}/pkg/usr/share/man/man7
}
