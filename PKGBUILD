# Maintainer: Rick van Lieshout <info@rickvanlieshout.com>

pkgname=tidal-hifi-git
pkgrel=2
pkgver=2.3.0.r0.g8fea526
pkgdesc="The web version of listen.tidal.com running in electron with hifi support thanks to widevine."
arch=(x86_64)
url="https://github.com/Mastermindzh/tidal-hifi"
license=("custom:MIT")
depends=(libxss nss gtk3)
makedepends=(npm git)
provides=(tidal-hifi)
conflicts=(tidal-hifi)

source=("git+https://github.com/Mastermindzh/tidal-hifi.git"
        "tidal-hifi.desktop")
sha512sums=('SKIP'
            '31cf40fb3ac81c4a64a8410a78e97c268a440577bce54347ce62f8a9566c8897f8083cd1e5afa40b0fbe9a149fc4fb4f29cad91a12e5b47cf8e300e56351a4f1')

pkgver() {
    cd "${srcdir}/${pkgname%-git}"
    git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd "${srcdir}/${pkgname%-git}"

    # install build dependencies
    npm install
}

build() {
    cd "${srcdir}/${pkgname%-git}"

    # We are not using the systems Electron as we need castlab's Electron.
    npm run build-arch
}

package() {
    cd "${srcdir}/${pkgname%-git}"

    install -d "${pkgdir}/opt/tidal-hifi/" "${pkgdir}/usr/bin" "${pkgdir}/usr/share/doc" "${pkgdir}/usr/share/licenses"

    cp -r dist/linux-unpacked/* "${pkgdir}/opt/tidal-hifi/"
    chmod +x "${pkgdir}/opt/tidal-hifi/tidal-hifi"

    ln -s "/opt/tidal-hifi/tidal-hifi" "${pkgdir}/usr/bin/tidal-hifi"

    install -Dm 644 "build/icon.png" "${pkgdir}/usr/share/pixmaps/tidal-hifi.png"
    install -Dm 644 "${srcdir}/tidal-hifi.desktop" "${pkgdir}/usr/share/applications/tidal-hifi.desktop"

    install -Dm 644 "README.md" "${pkgdir}/usr/share/doc/${pkgname}/README.md"
    install -Dm 644 "LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
    install -Dm 644 "build/icon.png" "${pkgdir}/usr/share/icons/hicolor/0x0/apps/tidal-hifi.png"
    ln -s "/opt/tidal-hifi/LICENSE.electron.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.electron.txt"
    ln -s "/opt/tidal-hifi/LICENSES.chromium.html" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSES.chromium.html"
}
