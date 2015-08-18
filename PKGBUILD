# Maintainer: Sergey Datsevich <s [dot] aur [at] 9dt [dot] de>

pkgname=yate-svn
_pkgname=yate
pkgver=5930
pkgrel=4
pkgdesc="Yet Another Telephony Engine"
arch=('i686' 'x86_64' 'ppc' 'armv6h')
url="http://yate.null.ro"
license=('GPL' 'MPL')
depends=('speex' 'postgresql-libs' 'libmysqlclient' 'php' 'sox' 'sqlite' 'spandsp')
makedepends=('doxygen' 'pkgconfig' 'subversion')
backup=('etc/yate/accfile.conf' 'etc/yate/ccongestion.conf' 'etc/yate/dsoundchan.conf'
	'etc/yate/h323chan.conf' 'etc/yate/lateroute.conf' 'etc/yate/mysqldb.conf'
	'etc/yate/queuesnotify.conf' 'etc/yate/sipfeatures.conf' 'etc/yate/wpcard.conf'
	'etc/yate/ysigchan.conf' 'etc/yate/amrnbcodec.conf' 'etc/yate/cdrbuild.conf'
	'etc/yate/enumroute.conf' 'etc/yate/heartbeat.conf' 'etc/yate/lksctp.conf'
	'etc/yate/openssl.conf' 'etc/yate/regexroute.conf' 'etc/yate/sqlitedb.conf'
	'etc/yate/yate.conf' 'etc/yate/ysipchan.conf' 'etc/yate/analog.conf'
	'etc/yate/cdrfile.conf' 'etc/yate/eventlogs.conf' 'etc/yate/isupmangler.conf'
	'etc/yate/mgcpca.conf' 'etc/yate/pbxassist.conf' 'etc/yate/regfile.conf'
	'etc/yate/ss7_lnp_ansi.conf' 'etc/yate/ysnmpagent.conf' 'etc/yate/cache.conf'
	'etc/yate/ciscosm.conf' 'etc/yate/extmodule.conf' 'etc/yate/jabberclient.conf'
	'etc/yate/mgcpgw.conf' 'etc/yate/pgsqldb.conf' 'etc/yate/register.conf'
	'etc/yate/subscription.conf' 'etc/yate/yiaxchan.conf' 'etc/yate/ysockschan.conf'
	'etc/yate/callcounters.conf' 'etc/yate/clustering.conf' 'etc/yate/fileinfo.conf'
	'etc/yate/jabberserver.conf' 'etc/yate/moh.conf' 'etc/yate/presence.conf'
	'etc/yate/rmanager.conf' 'etc/yate/tdmcard.conf' 'etc/yate/yjinglechan.conf'
	'etc/yate/ystunchan.conf' 'etc/yate/callfork.conf' 'etc/yate/cpuload.conf'
	'etc/yate/filetransfer.conf' 'etc/yate/javascript.conf' 'etc/yate/monitoring.conf'
	'etc/yate/providers.conf' 'etc/yate/sigtransport.conf' 'etc/yate/tonegen.conf'
	'etc/yate/yradius.conf' 'etc/yate/zapcard.conf' 'etc/yate/camel_map.conf'
	'etc/yate/dbpbx.conf' 'etc/yate/gvoice.conf' 'etc/yate/jbfeatures.conf'
	'etc/yate/mux.conf' 'etc/yate/queues.conf' 'etc/yate/sip_cnam_lnp.conf'
	'etc/yate/users.conf' 'etc/yate/yrtpchan.conf' 'etc/yate/zlibcompress.conf')
source=($pkgname::svn+http://voip.null.ro/svn/yate/trunk
	yate.service)
sha512sums=('SKIP'
	'48af107d62422ec80591a2844ca42bcb3da19580c6e6c32b501352045504015cccc21999fb068adf9027ab35aabe38fec7c0da19700c84745a3001ef7a5a2afc')
options=(!makeflags)

pkgver() {
	cd "$pkgname"
	svnversion
}

build() {
	cd "$pkgname"

	export LDFLAGS="${LDFLAGS//-Wl,--as-needed}"
	./autogen.sh
	./configure --prefix=/usr \
		--sysconfdir=/etc \
		--with-sqlite \
		--with-mysql \
		--without-libqt4
	make
	
	sed -i 's|"-I.*'${srcdir}'.*"|""|' yate-config
}

package() {
	cd "${pkgname}"
	
	make DESTDIR=${pkgdir} install

	install -D -m644 COPYING "${pkgdir}/usr/share/licenses/${_pkgname}/LICENSE"
	install -D -m644 "${srcdir}"/yate.service "${pkgdir}"/usr/lib/systemd/system/yate.service

	cd "${pkgdir}/etc/${_pkgname}"
	msg "Setting restrictive config files permissions"
	chmod -f 600 *.conf

	msg "Remove unneded files and docs"
	rm yate-qt4.conf

	rm -d "${pkgdir}/usr/lib/${_pkgname}/qt4"
	mv "${pkgdir}/usr/share/doc/${_pkgname}-$(${pkgdir}/usr/bin/yate-config --version)" "${pkgdir}/usr/share/doc/${_pkgname}"
	cd "${pkgdir}/usr/share/doc/${_pkgname}"
	rm -rf "api"
	rm *.html
}

