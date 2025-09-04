pkgname=lld
pkgver=21.1.0
pkgrel=1
pkgdesc="Linker from the LLVM project"
arch=('x86_64')
url="https://lld.llvm.org/"
license=('Apache-2.0 WITH LLVM-exception')
depends=(
    'gcc-libs'
    'llvm-libs'
    'zlib'
    'zstd'
)
makedepends=(
    'cmake'
    'llvm'
    'ninja'
    'python-sphinx'
)
source=(https://github.com/llvm/llvm-project/releases/download/llvmorg-${pkgver}/lld-${pkgver}.src.tar.xz
    https://github.com/llvm/llvm-project/releases/download/llvmorg-${pkgver}/llvm-${pkgver}.src.tar.xz
    https://github.com/llvm/llvm-project/releases/download/llvmorg-${pkgver}/libunwind-${pkgver}.src.tar.xz
    https://github.com/llvm/llvm-project/releases/download/llvmorg-${pkgver}/cmake-${pkgver}.src.tar.xz)
sha256sums=(0394c634edb6fa421b3690b042cfd5a42d7f7ab141aebedecb0b1d23ff882422
    0582ee18cb6e93f4e370cb4aa1e79465ba1100408053e1ff8294cef7fb230bd8
    bbee5d791ed693d57ff0668e7f150e43cb9616501fd20a48f96768b16cab2ca2
    528347c84c3571d9d387b825ef8b07c7ad93e9437243c32173838439c3b6028f)

prepare() {
    mv libunwind{-${pkgver}.src,}

    mv cmake{-${pkgver}.src,}
}

build() {
    cd lld-${pkgver}.src

    local cmake_args=(
        -G Ninja
        -B flarebird-build
        -D CMAKE_BUILD_TYPE=Release
        -D CMAKE_INSTALL_PREFIX=/usr
        -D LLVM_LIBDIR_SUFFIX=64
        -D CMAKE_INSTALL_DOCDIR=share/doc
        -D CMAKE_SKIP_INSTALL_RPATH=ON
        -D BUILD_SHARED_LIBS=ON
        -D LLVM_BUILD_DOCS=ON
        -D LLVM_ENABLE_SPHINX=ON
        -D LLVM_EXTERNAL_LIT=/usr/bin/lit
        -D LLVM_INCLUDE_TESTS=ON
        -D LLVM_LINK_LLVM_DYLIB=ON
        -D LLVM_MAIN_SRC_DIR="${srcdir}/llvm-${pkgver}.src"
        -D SPHINX_WARNINGS_AS_ERRORS=OFF
    )

    cmake "${cmake_args[@]}"

    cmake --build flarebird-build
}

package() {
    cd lld-${pkgver}.src

    DESTDIR=${pkgdir} cmake --install flarebird-build

    # https://bugs.llvm.org/show_bug.cgi?id=42455
    install -Dm644 -t ${pkgdir}/usr/share/man/man1 docs/ld.lld.1

    # Remove documentation sources
    rm -r ${pkgdir}/usr/share/doc/lld/html/{_sources,.buildinfo}


}
