cmake_minimum_required(VERSION 3.1.5)
project(vilib_catkin)

find_package(catkin REQUIRED)
find_package(OpenCV REQUIRED COMPONENTS core)
find_package(Eigen3 REQUIRED)
find_package(CUDA REQUIRED)

include(ExternalProject)
file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(vilib
  GIT_REPOSITORY https://github.com/uzh-rpg/vilib.git
  #  to reference specific commit, add a line like this
  # GIT_TAG 6b6a8495bb9148a415d98708707c10656b0b69a1
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cmake -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX} -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE} -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER} ../vilib
  )

catkin_package(
  LIBRARIES vilib
  INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  DEPENDS EIGEN3 OpenCV CUDA)


install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/vilib
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        FILES_MATCHING PATTERN "libvilib")
