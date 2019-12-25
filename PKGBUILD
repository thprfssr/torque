# Maintainer: Jingbei Li <i@jingbei.li>
# Contributor:  Martin C. Doege <mdoege at compuserve dot com>
pkgname="torque"
pkgver=6.1.1.1
pkgrel=1
linknum="3212"
pkgdesc='An open source resource manager providing control over batch jobs and distributed compute nodes'
arch=('i686' 'x86_64' 'aarch64')
url="http://www.adaptivecomputing.com/products/open-source/torque/"
license=('custom')
depends=('openssh' 'libxml2' 'tk' 'boost-libs')
makedepends=('make' 'boost')
backup=(var/spool/torque/server_name var/spool/torque/mom_priv/config var/spool/torque/serv_priv/{nodes,serverdb})
options=(!libtool)
install=torque.install
source=("torque-"$pkgver".tar.gz"::'http://www.adaptivecomputing.com/index.php?wpfb_dl='$linknum)
md5sums=('ec4979262e5f259e539873b208a191dd')
config_guess_url="http://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.guess;hb=HEAD"
config_sub_url="http://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.sub;hb=HEAD"

build() {
	cd "$srcdir/$pkgname-$pkgver"
	curl $config_guess_url > buildutils/config.guess
	curl $config_sub_url > buildutils/config.sub
	sed 's/\/sbin\/ldconfig/:/g' -i src/resmom/Makefile.{am,in}
	CPPFLAGS="$CPPFLAGS -O3 -fpermissive"
	./configure --prefix="/usr" --sbindir="/usr/bin" --disable-gui
	sed 's| -Werror | |g' -i `find . -name Makefile`
	make
}

package() {
	cd "$srcdir/$pkgname-$pkgver"
	make DESTDIR="$pkgdir" install
	# License
	install -D -m644 "${srcdir}/$pkgname-$pkgver/"PBS_License.txt "${pkgdir}/usr/share/licenses/torque/PBS_License.txt"
}
