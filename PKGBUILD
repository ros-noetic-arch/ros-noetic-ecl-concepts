# Maintainer: Timon Engelke <aur@timonengelke.de>
pkgdesc="ROS - Introduces a compile time concept checking mechanism that can be used most commonly to check for required functionality when passing template arguments."
url='https://wiki.ros.org/ecl_concepts'

pkgname='ros-melodic-ecl-concepts'
pkgver='0.62.2'
arch=('any')
pkgrel=1
license=('BSD')

ros_makedepends=(ros-melodic-ecl-license
  ros-melodic-ecl-config
  ros-melodic-ecl-type-traits
  ros-melodic-catkin)
makedepends=('cmake' 'ros-build-tools'
  ${ros_makedepends[@]})

ros_depends=(ros-melodic-ecl-license
  ros-melodic-ecl-config
  ros-melodic-ecl-type-traits)
depends=(${ros_depends[@]})

# Git version (e.g. for debugging)
# _tag=release/melodic/ecl_concepts/${pkgver}-${_pkgver_patch}
# _dir=${pkgname}
# source=("${_dir}"::"git+https://github.com/yujinrobot-release/ecl_core-release.git"#tag=${_tag})
# sha256sums=('SKIP')

# Tarball version (faster download)
_dir="ecl_core-${pkgver}/ecl_concepts"
source=("${pkgname}-${pkgver}.tar.gz"::"https://github.com/stonier/ecl_core/archive/${pkgver}.tar.gz")
sha256sums=('69d503109878e3a2c4f5be611fc9609078714899cae15fbf7f58119fdbd61e37')

build() {
  # Use ROS environment variables
  source /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/melodic/setup.bash ] && source /opt/ros/melodic/setup.bash

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  /usr/share/ros-build-tools/fix-python-scripts.sh -v 3 ${srcdir}/${_dir}

  # Build project
  cmake ${srcdir}/${_dir} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/melodic \
        -DPYTHON_EXECUTABLE=/usr/bin/python3 \
        -DPYTHON_INCLUDE_DIR=/usr/include/python3.7m \
        -DPYTHON_LIBRARY=/usr/lib/libpython3.7m.so \
        -DPYTHON_BASENAME=-python3.7m \
        -DSETUPTOOLS_DEB_LAYOUT=OFF
  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}/" install
}
