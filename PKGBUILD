# Maintainer: Rick van Lieshout <info@rickvanlieshout.com>

_pkgname=tidal-hifi
pkgname="$_pkgname-git"
pkgver=1.1.1
pkgrel=1
pkgdesc="The web version of listen.tidal.com running in electron with hifi support thanks to widevine."
arch=("x86_64")
url="https://github.com/Mastermindzh/tidal-hifi"
license=("custom:MIT")

depends=("libxss" "nss" "gtk3")
makedepends=("npm" "git")
provides=("$_pkgname")

source=("https://github.com/Mastermindzh/tidal-hifi/archive/$pkgver.zip"
        "${_pkgname}.desktop")
sha512sums=('ed6127627400cc8d1a614f3084eaed0ee9109cc4be99ad8865a7cd68c45d9bbd7b20404c394930af005320da5cc2221a8818a7cb48d4cccc73991e88320408e5'
            '35f38ac308b871c1822d7f6f760f2fb54c3748cf769822cb0f0dfb90f0f5754ba9316da5e903a0d2e9839de3a43ec76f238f3f2e44021956fa1da19142081349')

cdToPkg(){
    cd "tidal-hifi-$pkgver"
}

prepare() {
    cdToPkg

    # install build dependencies
    npm install
}

build() {
    cdToPkg

    # We are not using the systems Electron as we need castlab's Electron.
    npm run build-arch
}

package() {
    cdToPkg

    install -d "${pkgdir}/opt/${_pkgname}/" "${pkgdir}/usr/bin" "${pkgdir}/usr/share/doc" "${pkgdir}/usr/share/licenses"

    cp -r dist/linux-unpacked/* "${pkgdir}/opt/${_pkgname}/"
    chmod +x "${pkgdir}/opt/${_pkgname}/${_pkgname}"

    ln -s "/opt/${_pkgname}/${_pkgname}" "${pkgdir}/usr/bin/${_pkgname}"

    install -Dm 644 "build/icon.png" "${pkgdir}/usr/share/pixmaps/${_pkgname}.png"
    install -Dm 644 "${srcdir}/${_pkgname}.desktop" "${pkgdir}/usr/share/applications/${_pkgname}.desktop"

    install -Dm 644 "README.md" "${pkgdir}/usr/share/doc/${pkgname}/README.md"
    install -Dm 644 "LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
    ln -s "/opt/${_pkgname}/LICENSE.electron.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.electron.txt"
    ln -s "/opt/${_pkgname}/LICENSES.chromium.html" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSES.chromium.html"
}
