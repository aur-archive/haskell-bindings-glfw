# Maintainer: André van Delden <andre.van.deldenX@Xuni-bremen.de>

_hkgname=bindings-GLFW
pkgname=haskell-bindings-glfw
pkgver=3.0.3.2
pkgrel=1

pkgdesc="Low-level bindings to GLFW. These bindings are too low-level for normal use. For higher-level bindings, see GLFW-b."

url="http://hackage.haskell.org/package/${_hkgname}"
license=('BSD3')
arch=('i686' 'x86_64')
depends=('ghc' 'haskell-bindings-dsl>=1.0' 'haskell-bindings-dsl<1.1')
options=('strip' 'staticlibs')
source=(http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz)
install="${pkgname}.install"
sha512sums=('97cf4f00fdd001826a87b018c6a6c4f36f4ed06630f33f55bcdba5f6740c3abe307dec1346d7337b6765b6952d020062ab21937377460961faab8a8600cd106d')

build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    runhaskell Setup configure \
        -O \
        ${PKGBUILD_HASKELL_ENABLE_PROFILING:+-p } \
        --enable-split-objs \
        --enable-shared \
        --prefix=/usr \
        --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register   --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh \
        ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh \
        ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries

    # Documentation
    ln -s /usr/share/doc/${pkgname}/html \
        ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}

    # License
    install -D -m644 LICENSE ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/LICENSE

    runhaskell Setup copy --destdir=${pkgdir}
}
