# Maintainer: jsanchesdovichi [at] gmail (dot) com
pkgname=openblas-lapack-static
_PkgName=OpenBLAS
_pkgname=openblas
pkgver=0.3.23
# grep VERSION "${srcdir}/${_PkgName}-${pkgver}"/lapack-netlib/README.md | tail -n 1 | cut -d ' ' -f 2
_lapackver=3.11.0
_blasver=3.11.0
pkgrel=1
pkgdesc="Optimized BLAS library based on GotoBLAS2 1.13 BSD (providing blas, lapack, and cblas)"
arch=('x86_64')
url="http://www.openblas.net/"
license=('BSD')
depends=('gcc-libs')
makedepends=('perl'
'gcc-fortran'
"blas=${_blasver}" "lapack=${_lapackver}" "cblas=${_blasver}" "lapacke=${_lapackver}"
)
# provides=('openblas' "blas=${_blasver}" "lapack=${_lapackver}" "cblas=${_blasver}" "lapacke=${_lapackver}")
provides=('openblas')
conflicts=('openblas' 'blas' 'lapack' 'cblas' 'lapacke')
options=(!emptydirs staticlibs)
source=(${_PkgName}-${pkgver}.tar.gz::https://github.com/xianyi/${_PkgName}/archive/v${pkgver}.tar.gz)
sha256sums=('5d9491d07168a5d00116cdc068a40022c3455bf9293c7cb86a65b1054d7e5114')

# Add the following line to the _config variable if you want to set the number of make jobs
#  MAKE_NB_JOBS=2 \
_config="FC=gfortran USE_OPENMP=1 USE_THREAD=1 \
  USE_TLS=1 \
  NO_LAPACK=0 BUILD_LAPACK_DEPRECATED=1 \
  MAJOR_VERSION=${_lapackver:0:1}"

build(){
  cd "${srcdir}/${_PkgName}-${pkgver}"

  make ${_config} CFLAGS="${CFLAGS}" libs netlib shared
}

check(){
  cd "${srcdir}/${_PkgName}-${pkgver}"

  make ${_config} tests
}

package(){
  cd "${srcdir}/${_PkgName}-${pkgver}"

  make ${_config} PREFIX=/usr DESTDIR="${pkgdir}" install

  # Install license
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  # Symlink to provide blas, cblas, lapack and lapacke
  cd "${pkgdir}/usr/lib/"
  # BLAS
  ln -sf libopenblas.a libblas.a
  ln -sf libopenblas.so libblas.so
  ln -sf libopenblas.so libblas.so.${_blasver:0:1}
  ln -sf libopenblas.so libblas.so.${_blasver}
  # CBLAS
  ln -sf libopenblas.a libcblas.a
  ln -sf libopenblas.so libcblas.so
  ln -sf libopenblas.so libcblas.so.${_blasver:0:1}
  ln -sf libopenblas.so libcblas.so.${_blasver}
  # LAPACK
  ln -sf libopenblas.a liblapack.a
  ln -sf libopenblas.so liblapack.so
  ln -sf libopenblas.so liblapack.so.${_lapackver:0:1}
  ln -sf libopenblas.so liblapack.so.${_lapackver}
  # LAPACKE
  ln -sf libopenblas.a liblapacke.a
  ln -sf libopenblas.so liblapacke.so
  ln -sf libopenblas.so liblapacke.so.${_lapackver:0:1}
  ln -sf libopenblas.so liblapacke.so.${_lapackver}
}
