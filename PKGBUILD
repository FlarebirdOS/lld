pkgname=lld
pkgver=21.1.3
pkgrel=2
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
sha256sums=(8904e54475ca8426bc79b9278af1c3cccb40bf9958bd3f7d15f692f1b237d56f
    a80f2dbfa24a0c4d81089e6245936dcd0c662c90f643d1706bb44e7bc8338ff1
    ce4c70d9086bff55525cb4cc653306be46b668b7719b1d1a4da4d309902c9d38
    4db6f028b6fe360f0aeae6e921b2bd2613400364985450a6d3e6749b74bf733a)

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
