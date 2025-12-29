# Maintainer: Rick van Lieshout <info@rickvanlieshout.com>

pkgname=tidal-hifi-git
pkgrel=1
pkgver=6.0.0.r0.gfa8d7b1
pkgdesc="The web version of tidal.com running in electron with hifi support thanks to widevine. If the install fails use nvm to temporarily downgrade npm"
arch=(x86_64)
url="https://github.com/Mastermindzh/tidal-hifi"
license=("custom:MIT")
depends=(libxss nss gtk3 libxcrypt-compat libnotify)
makedepends=(git)
provides=(tidal-hifi)
conflicts=(tidal-hifi)

source=("git+https://github.com/Mastermindzh/tidal-hifi.git"
    "tidal-hifi.desktop"
    "tidal-hifi.xml")
sha512sums=('SKIP'
    'ab8fadd3eed29ad712324d2b1c91bf5a8a7e84af5ecc9b0ad9af3ecf5ffe31e048854e7346b0538c605fdb30059be1c051632fd314f0127390f3791edba6bdac'
    "e06fce55c2d9fcaeff514b97e8b003dca4c1a0aa8c8e14c3e3b99febbc2e8af7402d2e2009147f3f57a9b6447fafd23dd69e7b4de63cf43c5d67825836ebecb5")

getnvm() {
    if command -v nvm; then
        echo "nvm command found, using system version.."
    else

        if test -f "/usr/share/nvm/init-nvm.sh"; then
            echo "found init-nvm.sh in /usr/share/nvm, sourcing..."
            unset npm_config_prefix
            source "/usr/share/nvm/init-nvm.sh"
        else
            echo "nvm could not be found, installing"
            unset npm_config_prefix
            folderName=$(cat /dev/urandom | tr -cd 'a-f0-9' | head -c 12)
            git clone https://aur.archlinux.org/nvm.git "$folderName"
            cd "$folderName" || exit
            makepkg -si --asdeps
            source /usr/share/nvm/init-nvm.sh
            cd ../
            rm -rf "$folderName"
        fi
    fi
}

pkgver() {
    cd "${srcdir}/${pkgname%-git}" || exit
    git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    getnvm

    cd "${srcdir}/${pkgname%-git}" || exit

    # use correct nodejs/npm versions
    nvm install lts/jod
    nvm use lts/jod

    # install build dependencies
    npm install
}

build() {
    getnvm

    cd "${srcdir}/${pkgname%-git}" || exit

    # We are not using the systems Electron as we need castlab's Electron.
    npm run build-arch
}

package() {
    cd "${srcdir}/${pkgname%-git}" || exit

    install -d "${pkgdir}/opt/tidal-hifi/" "${pkgdir}/usr/bin" "${pkgdir}/usr/share/doc" "${pkgdir}/usr/share/licenses"

    cp -r dist/linux-unpacked/* "${pkgdir}/opt/tidal-hifi/"
    chmod +x "${pkgdir}/opt/tidal-hifi/tidal-hifi"

    ln -s "/opt/tidal-hifi/tidal-hifi" "${pkgdir}/usr/bin/tidal-hifi"

    install -Dm 644 "build/icon.png" "${pkgdir}/usr/share/pixmaps/tidal-hifi.png"
    install -Dm 644 "build/icon.png" "${pkgdir}/usr/share/icons/${pkgname%-git}/tidal-hifi.png"
    install -Dm 644 "build/icon.png" "${pkgdir}/usr/share/icons/hicolor/0x0/apps/tidal-hifi.png"
    install -Dm 644 "${srcdir}/tidal-hifi.desktop" "${pkgdir}/usr/share/applications/tidal-hifi.desktop"
    install -Dm 644 "${srcdir}/tidal-hifi.xml" "${pkgdir}/usr/share/mime/packages/tidal-hifi.xml"

    install -Dm 644 "README.md" "${pkgdir}/usr/share/doc/${pkgname}/README.md"
    install -Dm 644 "LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
    install -Dm 644 "build/icon.png" "${pkgdir}/usr/share/icons/hicolor/0x0/apps/tidal-hifi.png"

    ln -s "/opt/tidal-hifi/LICENSE.electron.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.electron.txt"
    ln -s "/opt/tidal-hifi/LICENSES.chromium.html" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSES.chromium.html"
}
