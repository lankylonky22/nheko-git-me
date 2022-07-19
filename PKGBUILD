# Maintainer: Konstantinos Sideris <siderisk at auth dot gr>
# Maintainer: Joseph Donofry <joe at joedonofry dot com>

pkgname=nheko-git
pkgver=0.9.3.r281.g1f971ae9
pkgrel=1
pkgdesc="Desktop client for the Matrix protocol"
arch=("i686" "x86_64" "aarch64")

url="https://github.com/Nheko-Reborn/nheko"
license=("GPL3")

depends=("qt5-base" "lmdb" "qt5-graphicaleffects" "qt5-multimedia" "qt5-svg" "qt5-quickcontrols2" "qt5-declarative" "qtkeychain-qt5" "cmark" "openssl" "hicolor-icon-theme" "gstreamer" "gst-plugins-base" "gst-plugins-good" "gst-plugins-bad" "gst-plugin-qmlgl" "libnice" "libolm" "spdlog" "curl" "libevent" "pkg-config")
makedepends=("git" "cmake" "gcc" "fontconfig" "qt5-tools" "nlohmann-json" "asciidoc")
optdepends=("qt-jdenticon")
provides=("nheko=${pkgver}")
conflicts=("nheko")

source=($pkgname::git+https://github.com/Nheko-Reborn/nheko.git#commit=1f971ae940e7e76e575881395df839431c539a6c)
md5sums=("SKIP")

prepare() {
  cd "$pkgname"
}

pkgver() {
    cd "$pkgname"
    git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
    cd "$pkgname"

    rm -f cmake/FindOlm.cmake

    cmake -H. -Bbuild -DHUNTER_ENABLED=OFF -DBUILD_SHARED_LIBS=OFF -DUSE_BUNDLED_MTXCLIENT=ON -DUSE_BUNDLED_COEURL=ON -DUSE_BUNDLED_LMDBXX=ON -DCMAKE_INSTALL_PREFIX=.deps/usr -DCMAKE_BUILD_TYPE=Release
    cmake --build build --config Release
}

package() {
    # Creating needed directories
    install -dm755 "$pkgdir/usr/bin"
    install -dm755 "$pkgdir/usr/share/pixmaps/"
    install -dm755 "$pkgdir/usr/share/applications/"

    # Program
    install -Dm755 "$pkgname/build/nheko" "$pkgdir/usr/bin/nheko"

    # Desktop launcher
    install -Dm644 "$srcdir/$pkgname/resources/nheko-256.png" "$pkgdir/usr/share/pixmaps/nheko.png"
    install -Dm644 "$srcdir/$pkgname/build/resources/nheko.desktop" "$pkgdir/usr/share/applications/nheko.desktop"

    # Man entry
    install -Dm644 "$srcdir/$pkgname/build/man/nheko.1" "$pkgdir/usr/share/man/man1/nheko.1"

    # Icons
    local icon_size icon_dir
    for icon_size in 16 32 48 64 128 256 512; do
        icon_dir="$pkgdir/usr/share/icons/hicolor/${icon_size}x${icon_size}/apps"
        install -d "$icon_dir"
        install -m644 "$srcdir/$pkgname/resources/nheko-${icon_size}.png" "$icon_dir/nheko.png"
    done
}
