# Maintainer: Daniel Hejduk <danielhejduk@disroot.org>
pkgname=opencompile-keys
pkgver=1.0
pkgrel=1
pkgdesc="Public keys for OpenCompile packages"
url="https://opencompile.github.io"
# we install arch specific keys to /etc so we cannot do arch=noarch
arch="all"
license="MIT"
replaces=""
options="!check" # No testsuite

_arch_keys="
	aarch64:danielhejduk@disroot.org-665c9d0d.rsa.pub
  x86_64:danielhejduk@disroot.org-665c9d0d.rsa.pub
	"

for _i in $_arch_keys; do
	source="$source ${_i#*:}"
done

_ins_key() {
	msg "- $2 ($1)"
	install -Dm644 "$srcdir"/$2 "$pkgdir"/etc/apk/keys/$2
}

_install_x86() {
	case "$1" in
	x86*) _ins_key $1 $2 ;;
	esac
}

_install_arm() {
	case "$1" in
	aarch64|arm*) _ins_key $1 $2 ;;
	esac
}

_install_ppc() {
	case "$1" in
	ppc*) _ins_key $1 $2 ;;
	esac
}

_install_s390x() {
	case "$1" in
	s390x) _ins_key $1 $2 ;;
	esac
}

_install_mips() {
	case "$1" in
	mips64) _ins_key $1 $2 ;;
	esac
}

_install_riscv() {
	case "$1" in
	riscv*) _ins_key $1 $2 ;;
	esac
}

package() {
	# copy keys for repos
	mkdir -p "$pkgdir"/etc/apk/keys
	for i in $_arch_keys; do
		_archs="${i%:*}"
		_key="${i#*:}"
		install -Dm644 "$srcdir"/$_key \
			"$pkgdir"/usr/share/apk/keys/$_key

		for _arch in ${_archs//,/ }; do
			mkdir -p "$pkgdir"/usr/share/apk/keys/$_arch
			ln -s ../$_key "$pkgdir"/usr/share/apk/keys/$_arch/

			case "$CARCH" in
			x86*) _install_x86 $_arch $_key ;;
			arm*|aarch64) _install_arm $_arch $_key ;;
			ppc*) _install_ppc $_arch $_key ;;
			s390x) _install_s390x $_arch $_key ;;
			mips*) _install_mips $_arch $_key ;;
			riscv*) _install_riscv $_arch $_key ;;
			esac
		done
	done
}

sha512sums="
0efb09560be5e82741f397c6050b94f55b66765b456284207906a6c4f020518a56aa8482bb829b63a43b9d4defa53577337455ffa5b1623d669a2771660d3af2  danielhejduk@disroot.org-665c9d0d.rsa.pub
"
