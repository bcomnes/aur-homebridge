# Maintainer: George Rawlinson <george@rawlinson.net.nz>
# Maintainer: Bret Comnes <bcomnes@gmail.com>

pkgname=homebridge
pkgver=1.8.4
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
sha512sums=('5abe50748fce0a9c42337add5f2b8f5313656eec22167827ba3f99b2b09b83970cc0a44c7cdf91446896d8b5126360431900b471252dffd4c5ead225a2fd7a43')
b2sums=('3f0cbb647c68e5622a94ce650f4126b5750dcd478186de875be61cce5b439348161af3a349aa5d31124407571bd74e3ceb5149b9476d074be73dea0672d37573')

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
