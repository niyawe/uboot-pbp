# Maintainer: Nadia Holmquist Pedersen <nadia@nhp.sh>

pkgname=uboot-pbp
pkgver=2021.01rc5
_ubootver=v$(echo "$pkgver" | sed "s/rc/-rc/")
pkgrel=1
pkgdesc='U-boot for the Pinebook Pro.'
arch=('aarch64')
url='https://www.denx.de/wiki/U-Boot'
license=('GPL2')
install=${pkgname}.install
makedepends=('bc' 'make' 'gcc' 'dtc' 'uboot-tools' 'python')

_patches=(
	"0002-Correct-boot-order-to-be-USB-SD-eMMC.patch"
	"0003-Enable-the-power-LED-during-early-startup.patch"
	"0005-PBP-Fix-Panel-reset.patch"
)

source=(
	"git+https://github.com/u-boot/u-boot.git#tag=${_ubootver}"
	"config"
    "bl31.elf"
	"${_patches[@]}"
)
sha256sums=('SKIP'
            '5b7eef9c4467e22a26919a6882f3f188a354bd0d835282b4301b187fefd70092'
            '56516fa4f6aab2730b24d9018488890497dc8c19af9b126e3e434c6555ad1dfa'
            '01f2389dec80b28024aa8f14133b62865dfac0285d0101ea0d7506639294e875'
            'fe82a9822c2c3ade6c5908df1b10b5102626c7bfcf59d8cdfc6f89aa33fd81c9'
            'c3ea09a18b766a3ce0728234b097b29e2ed610c7f04b138b7fba42e118a7ae33')

prepare() {
	cd "${srcdir}/u-boot"

	# Patches almost entirely based on dhivael's u-boot repository at https://git.eno.space/pbp-uboot.git
	for patch in "${_patches[@]}"; do
		patch -Np1 -i "${srcdir}/$patch"
	done

	cp "${srcdir}/config" .config
}

build() {
	unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
	make -C "${srcdir}"/u-boot BL31="${srcdir}"/bl31.elf
}

package() {
	cd "${srcdir}/u-boot"

	install -Dm 644 idbloader.img "${pkgdir}"/boot/idbloader.img
	install -Dm 644 u-boot.itb "${pkgdir}"/boot/u-boot.itb
}
