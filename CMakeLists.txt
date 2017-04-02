cmake_minimum_required(VERSION 2.6.0)

# pull in the pods macros. See cmake/pods.cmake for documentation
set(POD_NAME kinect)
include(cmake/pods.cmake)

# automatically build LCM types.  This also defines a number of CMake
# variables, see cmake/lcmtypes.cmake for details
include(cmake/lcmtypes.cmake)
lcmtypes_build()

# include libfreenect
pkg_check_modules(FNECT REQUIRED libfreenect)
include_directories(${LCMTYPES_INCLUDE_DIRS} ${FNECT_INCLUDE_DIRS})

# build LCM kinect utility
add_subdirectory(src/kinect-lcm)

# kinect utility library
add_subdirectory(src/kinect-utils)

# simple GLUT visualization tool
add_subdirectory(src/kinect-glview)

# bot2-vis renderer and viewer
add_subdirectory(src/kinect-renderer)

#add_subdirectory(src/kinect-openni-lcm)

add_subdirectory(src/tools)
