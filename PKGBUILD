# Maintainer: Kuba Kuźma <kuba@jah.pl>

# Modified from postgresql-9.4 package provided by:
# Matthew Carter <m@ahungry.com>

pkgname=postgresql-9.5
pkgver=9.5.12
pkgrel=1
arch=('i686' 'x86_64')
url="http://www.postgresql.org/"
pkgdesc="A powerful, open source object-relational database system"
license=('custom:PostgreSQL')
conflicts=('postgresql' 'postgresql-libs' 'postgresql-testing' 'postgresql-client')
provides=("postgresql=$pkgver" "postgresql-libs=$pkgver" "postgresql-client=$pkgver")
depends=('libxml2' 'krb5' 'openssl>=1.0.0' 'readline>=6.0')
makedepends=('python2' 'perl' 'tcl>=8.6.0')
optdepends=('python2: for PL/Python support'
            'perl: for PL/Perl support'
            'tcl: for PL/Tcl support'
            'postgresql-old-upgrade: upgrade from previous major version using pg_upgrade')
install=postgresql.install
source=(https://ftp.postgresql.org/pub/source/v${pkgver}/postgresql-${pkgver}.tar.bz2
        postgresql-run-socket.patch
        postgresql.pam postgresql.logrotate
        postgresql.service postgresql.tmpfiles.conf postgresql-check-db-dir)
sha256sums=('02e86f5c66467731bbec18fde96e0daf38c13c9141d8e7d41be663ab6fa6f698'
            '8538619cb8bea51078b605ad64fe22abd6050373c7ae3ad6595178da52f6a7d9'
            '57dfd072fd7ef0018c6b0a798367aac1abb5979060ff3f9df22d1048bb71c0d5'
            '6abb842764bbed74ea4a269d24f1e73d1c0b1d8ecd6e2e6fb5fb10590298605e'
            'c69b614731fdd3f4e127540814f5999f423363523c5209e252e19b1d4362ad68'
            '7e086d70e0dcaa6ce45693b4e826ce92d770192b3aff5642319b1ef279d88dc4'
            '20a8716745ff3690275ff088ea394d397063d4c5e159ca26d0d91462146bc74b')

build() {
  cd "${srcdir}/postgresql-${pkgver}"

  patch -Np1 < ../postgresql-run-socket.patch

  ./configure --prefix=/usr \
  --mandir=/usr/share/man \
  --datadir=/usr/share/postgresql \
  --sysconfdir=/etc \
  --with-krb5 \
  --with-gssapi \
  --with-libxml \
  --with-openssl-1.0 \
  --with-perl \
  --with-python PYTHON=/usr/bin/python2 \
  --with-tcl \
  --with-pam \
  --with-system-tzdata=/usr/share/zoneinfo \
  --enable-nls \
  --enable-thread-safety

  make
  make -C contrib
}

package() {
  cd "${srcdir}/postgresql-${pkgver}"

  # install license
  install -D -m644 COPYRIGHT "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  make DESTDIR="${pkgdir}" install
  make -C contrib DESTDIR="${pkgdir}" install

  install -D -m644 "${srcdir}/postgresql.tmpfiles.conf" \
    "${pkgdir}/usr/lib/tmpfiles.d/postgresql.conf"
  install -D -m644 "${srcdir}/postgresql.service" \
    "${pkgdir}/usr/lib/systemd/system/postgresql.service"
  install -D -m755 "${srcdir}/postgresql-check-db-dir" \
    "${pkgdir}/usr/bin/postgresql-check-db-dir"

  install -D -m644 "${srcdir}/postgresql.pam" \
    "${pkgdir}/etc/pam.d/postgresql"

  install -D -m644 "${srcdir}/postgresql.logrotate" \
    "${pkgdir}/etc/logrotate.d/postgresql"
}
