# Maintainer: nic <nicolasciampa2001@gmail.com>
# Personal-repo PKGBUILD: builds the upstream "Whether" COSMIC applet from a
# pinned release tag. Bump pkgver + updpkgsums to update.
pkgname=ncos-weather-applet
_upstream=cosmic-ext-whether
_appid=com.github.nwxnw.cosmic-ext-whether
pkgver=0.6.0
pkgrel=1
pkgdesc="COSMIC panel applet with weather forecasts, air quality, and alerts (Whether)"
arch=('x86_64' 'aarch64')
url="https://github.com/nwxnw/cosmic-ext-whether"
license=('GPL-3.0-only')
depends=('libxkbcommon' 'wayland') # wayland: libwayland-client is dlopen'd at runtime
makedepends=('cargo' 'just' 'git') # git: libcosmic is a cargo git dependency
provides=('cosmic-ext-whether')
conflicts=('cosmic-ext-whether')
options=('!lto') # cargo already does its own LTO; avoid makepkg injecting -flto
source=("$_upstream-$pkgver.tar.gz::$url/archive/refs/tags/v$pkgver.tar.gz")
sha256sums=('3b71b153dee213be85934180a44143510efbdb6892dfeec811a623bd5742b285')

prepare() {
    cd "$_upstream-$pkgver"
    # Upstream's Cargo.lock is complete as of 0.6.0 (0.3.2 was missing the
    # libcosmic build_helpers entry), so a locked fetch works again.
    cargo fetch --locked
}

build() {
    cd "$_upstream-$pkgver"
    just build-release --offline
}

package() {
    cd "$_upstream-$pkgver"
    install -Dm755 "target/release/$_upstream" "$pkgdir/usr/bin/$_upstream"
    # Upstream's `just install` also ships a $_appid.sh wrapper and rewrites
    # Exec= — both are Flatpak/per-user workarounds, deliberately skipped here.
    # Since 0.6.0 upstream ships only the symbolic icon (the colored app icon
    # lives in the store listing, not the source tree).
    install -Dm644 "data/$_appid.desktop" \
        "$pkgdir/usr/share/applications/$_appid.desktop"
    install -Dm644 "data/$_appid.metainfo.xml" \
        "$pkgdir/usr/share/metainfo/$_appid.metainfo.xml"
    install -Dm644 "data/$_appid-symbolic.svg" \
        "$pkgdir/usr/share/icons/hicolor/scalable/apps/$_appid-symbolic.svg"
}
