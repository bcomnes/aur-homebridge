# Maintainer: George Rawlinson <george@rawlinson.net.nz>
# Maintainer: Bret Comnes <bcomnes@gmail.com>

pkgname=homebridge
pkgver=1.10.0
pkgrel=1
pkgdesc='HomeKit support for the impatient'
arch=('any')
url='https://github.com/homebridge/homebridge'
license=('Apache')
depends=('nodejs')
makedepends=('npm' 'jq')
optdepends=('homebridge-config-ui-x: for web-based management tool')
options=('!emptydirs' '!strip')
source=("https://registry.npmjs.org/$pkgname/-/$pkgname-$pkgver.tgz")
noextract=("$pkgname-$pkgver.tgz")
sha512sums=('943ee3045b06841b85d4727dfa2bece409c00c42517c729760b399934e197c2ffa2fb133e05b654096c43cc06cf7c2c01f44225e65e60ae66753e2ff8c22901e')
b2sums=('86b7809522bb31f971caab175e87455ed662beeaab0a06bff201d0951bd84a42d49f1c3bdcbb4d3d6a3526d16d89bd2c23d2e36dc0111b509ea3e0c81aca895f')

package() {
  npm install \
    --global \
    --cache "${srcdir}/npm-cache" \
    --prefix "$pkgdir/usr" \
    --no-audit --no-fund --no-update-notifier \
    "$srcdir/$pkgname-$pkgver.tgz"

    # Clean up srcdir references
    # https://wiki.archlinux.org/title/Node.js_package_guidelines#Package_contains_reference_to_$srcdir/$pkgdir
    find "$pkgdir" -name package.json -print0 | xargs -r -0 sed -i '/_where/d'

    local tmppackage="$(mktemp)"
    local pkgjson="$pkgdir/usr/lib/node_modules/$pkgname/package.json"
    jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
    mv "$tmppackage" "$pkgjson"
    chmod 644 "$pkgjson"

    find "$pkgdir" -type f -name package.json | while read pkgjson; do
      local tmppackage="$(mktemp)"
      jq 'del(.man)' "$pkgjson" > "$tmppackage"
      mv "$tmppackage" "$pkgjson"
      chmod 644 "$pkgjson"
    done
}
