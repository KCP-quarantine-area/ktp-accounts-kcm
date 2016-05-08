
# include global config
#source ../_buildscripts/${current_repo}-${_arch}-cfg.conf

pkgname=ktp-accounts-kcm
pkgver=16.04.0
pkgrel=1
pkgdesc="KCM Module for configuring Telepathy Instant Messaging Accounts"
arch=('x86_64')
url="https://projects.kde.org/projects/extragear/network/telepathy/ktp-accounts-kcm"
license=('GPL')
depends=('kcmutils' 'kio' 'ki18n' 'kwidgetsaddons' 'kcoreaddons' 'kconfigwidgets' 'kiconthemes' 'telepathy-haze'
         'kitemviews' 'ktp-common-internals' 'telepathy-gabble' 'kaccounts-providers' 'purple-skypeweb' 'purple-facebook')
makedepends=('extra-cmake-modules' 'kdoctools' 'git' 'boost' 'intltool')
groups=('kde-telepathy')
conflicts=('kf5-ktp-accounts-kcm')
replaces=('kf5-ktp-accounts-kcm')
source=("https://github.com/KDE/ktp-accounts-kcm/archive/v16.04.0.tar.gz"
"crazy.patch")
md5sums=('1385ddbaa358ac6c1bae6fea2e97d00d'
         'b5c4bad875d8245cf107cded421eb3ae')

prepare() {
  cd ${pkgname}-${pkgver}
  sed -i -e 's|add_subdirectory (sipe)|#add_subdirectory (sipe)|' plugins/CMakeLists.txt
 patch -p1 -i ${srcdir}/crazy.patch
 }

build() {
  mkdir -p build
  cd build

  cmake ../${pkgname}-${pkgver} \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DLIB_INSTALL_DIR=lib \
    -DSYSCONF_INSTALL_DIR=/etc \
    -DQML_INSTALL_DIR=/usr/lib/qt5/qml \
    -DPLUGIN_INSTALL_DIR=/usr/lib/qt5/plugins \
    -DKDE_INSTALL_USE_QT_SYS_PATHS=ON \
    -DBUILD_TESTING=OFF 
  make
}

package() {
  cd build
  make DESTDIR=${pkgdir} install
  # remove sipe protocol from accounts view
  rm ${pkgdir}/usr/share/accounts/providers/kde/ktp-sipe.provider
  rm ${pkgdir}/usr/share/accounts/providers/kde/ktp-sipe-haze.provider
}
