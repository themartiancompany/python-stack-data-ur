# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>

_pkg=stack-data
_Pkg=stack_data
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
pkgname="${_py}-${_pkg}"
pkgver=0.6.3
pkgrel=4
_pkgdesc=(
  'Extract data from python stack'
  'frames and tracebacks for'
  'informative displays'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'any'
)
_http="https://github.com"
_ns="alexmojaki"
url="${_http}/${_ns}/${_Pkg}"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-asttokens"
  "${_py}-executing"
  "${_py}-pure-eval"
)
makedepends=(
  'git'
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools-scm"
  "${_py}-wheel"
)
checkdepends=(
  'cython'
  "${_py}-littleutils"
  "${_py}-pygments"
  "${_py}-pytest"
  "${_py}-typeguard"
)
source=(
  "git+${url}.git#tag=v${pkgver}"
)
b2sums=(
  'e5bbc954348a78aeea4faeb5c5da8d4c2c14ae3c4079862ab2485275a1ffcc29f05fef1883336c1a05d3b46ec7a9c0742f0d387cb9b51dca8b931877f8ba7995'
)

build() {
  cd \
    "${_Pkg}"
  "${_py}" \
    -m \
      "build" \
      --wheel \
      --skip-dependency-check \
      --no-isolation
}

check() {
  cd \
    "${_Pkg}"
  PYTHONPATH="${PWD}/${_Pkg}:${PYTHONPATH}" \
    pytest
}

package() {
  local \
    _site_packages
  _site_packages="$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")"
  cd \
    "${_Pkg}"
  "${_py}" \
    -m \
      "installer" \
      --destdir="${pkgdir}" \
      "dist/"*".whl"
  # Symlink license file
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${_site_packages}/${_pkg}-${pkgver}.dist-info/LICENSE.txt" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.txt"
}
