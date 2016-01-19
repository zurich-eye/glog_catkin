
# in case you are on ubuntu 14.04 you have to upgrade cmake:
# sudo add-apt-repository ppa:george-edison55/cmake-3.x

cmake_minimum_required(VERSION 3.0)
project(glog_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(glog_src
  GIT_REPOSITORY https://github.com/google/glog.git
  UPDATE_COMMAND ""
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
    -DBUILD_SHARED_LIBS:BOOL=ON
    -DBUILD_EXAMPLES:BOOL=OFF
    -DCPP11_NO_BOOST:BOOL=ON
    BUILD_COMMAND ${CMAKE_COMMAND} --build ./ --target install
    INSTALL_COMMAND ""
)
ExternalProject_CatkinInstall(glog_src)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} glog_src)

cs_install()

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS glog-extras.cmake)
