cmake_minimum_required(VERSION 2.8.3)
project(xerces_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)
file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)
ExternalProject_Add(xerces_src
  URL http://www-us.apache.org/dist//xerces/c/3/sources/xerces-c-3.2.1.tar.gz
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cd ../xerces_src && cmake . -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX} -DBUILD_SHARED_LIBS:BOOL=true -DGFLAGS_NAMESPACE:STRING=google
-DCMAKE_BUILD_TYPE:STRING=Release
  BUILD_COMMAND cd ../xerces_src && make -j8	
  INSTALL_COMMAND cd ../xerces_src && make install -j8
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} xerces_src)

cs_install()
cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include)
