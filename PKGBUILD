# Maintainer: Alex Szczuczko <alex at szc dot ca>

pkgname=papersplease-beta
pkgver=0.5.13
pkgrel=6
pkgdesc="Assume the role of an immigration inspector for the communist state of Arstotzka (uses Wine)"
url='http://dukope.com/#ppl'
arch=('any')
license=('custom')
depends=('wine' 'wine_gecko' 'lib32-libldap' 'lib32-mpg123' 'lib32-sdl')
makedepends=('icoutils')
options=(!strip)
install="$pkgname.install"
source=("$pkgname.desktop"
        "launch-$pkgname.sh"
        "http://dl.dropboxusercontent.com/u/8663530/ppl/$pkgver/PapersPlease-$pkgver-Win.zip")
noextract=("PapersPlease-$pkgver-Win.zip")
sha256sums=('76abbe5f97dea8bf51b5ba594355b7c3df251dffa3e06eef46a94df9280bc55b'
            '709d665b6470b0aba0f29ce0ae062f32728f15693df8966db7205436fb8bae1d'
            '53c9c17e3fb5162993a372dcbfe1be327b5a1d06fc68dbfa2d2a5243cdfd0b8d')

# Disable compression of the package
PKGEXT='.tar'

build() {
    cd "$srcdir"

    # Extract the zip ourselves to keep the contents seperate
    mkdir -p $pkgname-zip
    bsdtar -x -C $pkgname-zip -f PapersPlease-$pkgver-Win.zip

    # Extract icon from exe (requires icoutils)
    wrestool -x -t14 -n101 $pkgname-zip/PapersPlease.exe -o temp.ico
    icotool -x -w256 -h256 -b32 -o $pkgname.png temp.ico || true
}

package() {
    cd "$srcdir"

    # Main files
    mkdir -p "$pkgdir/opt/$pkgname/game/"
    cp -R $pkgname-zip/* "$pkgdir/opt/$pkgname/game/"

    # The readme is the closest thing the game has to a license right now.
    mkdir -p "$pkgdir/usr/share/licenses/$pkgname/"
    ln -s -T "/opt/$pkgname/game/Readme.txt" "$pkgdir/usr/share/licenses/$pkgname/Readme.txt"

    # Launcher
    install -Dm755 "launch-$pkgname.sh" "$pkgdir/opt/$pkgname/launch-$pkgname.sh"
    mkdir -p "$pkgdir/usr/bin/"
    ln -s -T "/opt/$pkgname/launch-$pkgname.sh" "$pkgdir/usr/bin/$pkgname"

    # Icon
    install -Dm644 $pkgname.png "$pkgdir/usr/share/icons/$pkgname.png"

    # .desktop File
    install -Dm644 "$pkgname.desktop" "$pkgdir/usr/share/applications/$pkgname.desktop"
}
