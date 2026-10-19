# ncos-weather-applet

Packaging for [Whether](https://github.com/nwxnw/cosmic-ext-whether)
(`cosmic-ext-whether`) — a COSMIC panel applet with weather forecasts, air
quality, and alerts — as the `ncos-weather-applet` pacman package for the
[ncos repo](https://github.com/exothermic88/ncos-repo).

Unlike the other ncos package dirs, this one holds only the PKGBUILD: the
source is downloaded from the pinned upstream release tag (`v$pkgver`).

## Build

```sh
makepkg -srf
```

## Update to a new upstream release

1. Bump `pkgver` in the PKGBUILD (and reset `pkgrel=1`).
2. `updpkgsums`
3. `makepkg -srf`

## Publish to the ncos repo

```sh
cp ncos-weather-applet-<ver>-<rel>-x86_64.pkg.tar.zst ~/claude_projects/ncos-repo/x86_64/
cd ~/claude_projects/ncos-repo
./rename   # repo-add + db rename
git add . && git commit -m "Add ncos-weather-applet <ver>-<rel>"
./push
```

Do not ship the `-debug` package.
