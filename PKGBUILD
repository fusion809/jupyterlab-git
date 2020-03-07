# Maintainer: Brenton Horne <brentonhorne77@gmail.com>

pkgname=jupyterlab-git
pkgver=18280.git.aa3f0a0
pkgrel=1
pkgdesc="JupyterLab computational environment"
arch=(any)
url="https://github.com/jupyterlab/jupyterlab"
license=(custom)
makedepends=(python-setuptools nodejs python-recommonmark jsx-lexer)
depends=(jupyterlab_server)
source=("git+https://github.com/jupyterlab/jupyterlab.git"
jupyter-lab.desktop)
sha256sums=('SKIP'
            'd7ed2287b823a78b7fe05194180ad9b4602657d5e32b8ed548418039451c0434')

pkgver() {
    cd $srcdir/jupyterlab
    no=$(git rev-list --count HEAD)
    hash=$(git log | head -n 1 | cut -d ' ' -f 2 | head -c 7)
    printf "${no}.git.${hash}"
}

build() {
  cd $srcdir/jupyterlab
  python setup.py build 
  cd docs
  make html
}

package() {
  cd $srcdir/jupyterlab
  python setup.py install --skip-build --root="$pkgdir" --optimize=1

  install -Dm644 LICENSE "$pkgdir"/usr/share/licenses/$pkgname/LICENSE

  # symlink to fix assets
  install -d "$pkgdir"/usr/share/jupyter
  ln -s `python -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())"`/jupyterlab "$pkgdir"/usr/share/jupyter/lab

  install -d "$pkgdir"/usr/share/{pixmaps,doc/${pkgname}}
  install -Dm644 examples/notebook/jupyter.png "$pkgdir"/usr/share/pixmaps/jupyter.png
  install -Dm755 $srcdir/jupyter-lab.desktop "$pkgdir"/usr/share/applications/jupyter-lab.desktop
  cp -r docs/build/html/* ${pkgdir}/usr/share/doc/${pkgname}
}
