# Maintainer: Maxim Kurnosenko <asusx2@mail.ru>
# Contributor: RonaldMcDaddy <wannes.demeyer@protonmail.com>
# Contributor: Tinh Truong <xuantinh at gmail dot com>
# Contributor: Cedric Sougne <cedric@sougne.name>
# Contributor: untseac
# Contributor: siasia
# Contributor: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Contributor: JuliusTZM <julius dot tzm at gmail dot com>
# Contributor: Raleigh Matlock <raleighman2@gmail.com>

pkgname=oracle-xe
pkgver=18c
pkgrel=1
pkgdesc="a non free DBMS"
url="http://www.oracle.com/"
license=('custom')
arch=('x86_64')
conflicts=('oracle-xe')
provides=('oracle-xe')
options=('!strip')
depends=('libaio>=0.3.112-2' 'gcc>=9.3.0-1' 'binutils>=2.34-2.1' 'make>=4.3-1' 'glibc>=2.31-2' 'bc' 'net-tools')
install='oracle-xe.install'
source=(
        'manual://download/file/from/oracle/page/oracle-database-xe-18c-1.0-1.x86_64.rpm'
        'oracle_env.csh'
        'oracle_env.sh'
        'oracle-xe-18c'
        'oracle-xe-18c.conf'
        'oracle-xe-18c.ld.so.conf'
        'oracle-xe.service'
)

DLAGENTS+=('manual::/usr/bin/echo The source file for this package needs to be downloaded manually, since it requires a login and is not redistributable. Please visit http://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html')

 md5sums=(
        '56af7b3b58abdee310aa74f8f88e16ad'
        '66416a216ac1f7597f72c6b7aee48ac3'
        'dc3e9d178c4245d0e990051093a92483'
        'f8ff664b58569552d617d9bb8df05a70 '
        '9ce538757edbdb49847677ef6520de5b'
        '9f0c964a1b046e8d930549915b468e35'
        '6bcf64b585c2fa791137e7df7426851c'
 )

build() {
    cd $srcdir
#    bsdtar -xf oracle-database-xe-18c-1.0-1.x86_64.rpm
}

package() {
    cd $srcdir

    mkdir -p $pkgdir/etc/rc.d
    cp $srcdir/oracle-xe-18c $pkgdir/etc/rc.d/
    chmod +x $pkgdir/etc/rc.d/oracle-xe-18c

    #Fix for ***[FATAL] [DBT-50000] Unable to check for available memory.****
    corr1="s_-sampleSchema_-J-Doracle.assistants.dbca.validate.ConfigurationParams=false -sampleSchema_g"
    sed -i "${corr1}" $pkgdir/etc/rc.d/oracle-xe-18c

    mkdir -p $pkgdir/etc/sysconfig
    cp $srcdir/oracle-xe-18c.conf $pkgdir/etc/sysconfig

    mkdir -p $pkgdir/opt
    mv $srcdir/opt/oracle $pkgdir/opt

    find $pkgdir -exec chmod 755 {} \;

    # Export environment variables
    mkdir -p $pkgdir/etc/profile.d
    cp $srcdir/oracle_env.* $pkgdir/etc/profile.d/
    chmod +x $pkgdir/etc/profile.d/oracle_env.*

    # Desktop files
    cp -a $srcdir/usr $pkgdir

    # LD_LIBRARY_PATH
    mkdir -p $pkgdir/etc/ld.so.conf.d/
    cp $srcdir/oracle-xe-18c.ld.so.conf $pkgdir/etc/ld.so.conf.d/oracle-xe-18c.conf

    # License
    mkdir -p $pkgdir/usr/share/licenses/custom/$pkgname
    cp $srcdir/usr/share/doc/oracle-xe-18c/LICENSE $pkgdir/usr/share/licenses/custom/$pkgname

    # For systemd
    mkdir -p $pkgdir/etc/systemd/system
    cp $srcdir/oracle-xe.service $pkgdir/etc/systemd/system
}
