# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: damir <damir@archlinux.org>

pkgname=uim
pkgver=1.8.9
pkgrel=1
pkgdesc='Multilingual input method library'
url='https://github.com/uim/uim'
license=('custom:BSD')
arch=('x86_64')
depends=('libxft' 'libedit' 'm17n-lib')
makedepends=('intltool' 'gettext' 'gtk2' 'gtk3' 'qt5-x11extras' 'skk-jisyo' 'cmake' 'extra-cmake-modules')
optdepends=('qt5-x11extras: immodule and helper applications'
            'gtk2: immodule and helper applications'
            'gtk3: immodule and helper applications'
            'skk-jisyo: input method'
            'anthy: input method')
source=(
    "https://github.com/${pkgname}/${pkgname}/releases/download/${pkgver}/${pkgname}-${pkgver}.tar.bz2"
    "skk-utf8.patch"
)
sha256sums=('dbbd983768bf748449551644f330dbebe859bfeb6f024fea6697ac75131c7aa4'
            '538da86eca36424b0d6f323e0074d24030fcf6be7f359fd49be0b0670efa6672')

build() {
	cd "${srcdir}/${pkgname}-${pkgver}/scm"
    for f in japanese-act.scm japanese-azik.scm japanese-custom.scm japanese-kana.scm japanese-kzik.scm japanese.scm skk.scm skk-custom.scm
     do
      new="$(echo $f | sed 's/\.scm$/-utf8.scm/')"
      iconv -f EUC-JP -t UTF-8 < "$f" | sudo tee "$new" > /dev/null
    done

	cd "${srcdir}/${pkgname}-${pkgver}"
    patch -Np0 -b -i "${srcdir}/skk-utf8.patch"

	cd "${srcdir}/${pkgname}-${pkgver}"
    ./autogen.sh

	cd "${srcdir}/${pkgname}-${pkgver}"

	CFLAGS+=' -fcommon' # https://wiki.gentoo.org/wiki/Gcc_10_porting_notes/fno_common

	./configure \
		--prefix=/usr \
		--libexecdir=/usr/lib/uim \
		--with-qt5-immodule \
		--with-skk \
		--enable-notify=libnotify \

	make
}

package() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	make DESTDIR="${pkgdir}" install -j1 # FS#41112
	install -D -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/COPYING"
}
