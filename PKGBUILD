# Contributor: Hamed Salehi <lachfome@gmail.com>
# Maintainer: Francesco Colista <francesco.colista@gmail.com>
pkgname=ipsec-openswan
pkgver=2.6.38
pkgrel=0
pkgdesc="Open Source implementation of IPsec for the Linux operating system"
url="http://www.openswan.org"
license=('GPL' 'custom')
arch=('i686' 'x86_64')
depends=('iproute' 'gmp' 'perl')
backup=(etc/ipsec.conf \
        etc/ipsec.d/policies/{block,clear,clear-or-private,private,private-or-clear})
source=(http://www.openswan.org/download/openswan-$pkgver.tar.gz
	openswan
        openswan.service
        compile.patch)

build() {
  # Create /etc/rc.d for init script, and license directory
  mkdir -p $pkgdir/{etc/rc.d,usr/share/licenses/openswan}

  cd $srcdir/openswan-$pkgver
  patch -p1 -i $srcdir/compile.patch

  # Change install paths to Arch defaults
  sed -i 's|/usr/local|/usr|;s|libexec/ipsec|lib/openswan|' Makefile.inc

  make USE_XAUTH=true USE_OBJDIR=true programs || return 1
  make DESTDIR=$pkgdir install

  # Change permissions in /var
  chmod 755 $pkgdir/var/run/pluto
  
  # Copy License
  cp LICENSE $pkgdir/usr/share/licenses/openswan
  
  # Install init script
  install -Dm755 $srcdir/openswan $pkgdir/etc/rc.d/openswan
  install -Dm644 $srcdir/openswan.service $pkgdir/usr/lib/systemd/system/openswan.service
  mkdir $pkgdir/usr/lib/systemd/scripts/
  cp $pkgdir/etc/rc.d/ipsec $pkgdir/usr/lib/systemd/scripts/ipsec
  # fix manpages
  mv $pkgdir/usr/man $pkgdir/usr/share/


}

md5sums=('13073eb5314b83a31be88e4117e8bbcd'
         '43a8d702d517320c551bab9fb6d088c6'
         'd8b465c10838c72e31329d65011002b6'
         '5540437bb334873da646e21ac9caa963')
