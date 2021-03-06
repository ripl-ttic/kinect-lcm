add_definitions(-Wall)

add_library(kinect-utils SHARED kinect-calib.c)

set_target_properties(kinect-utils PROPERTIES SOVERSION 1)

pods_install_headers(kinect-calib.h 
    kinect-utils.h
    DESTINATION kinect)

pods_install_libraries(kinect-utils)

pods_install_pkg_config_file(kinect-utils
    LIBS -lkinect-utils
    VERSION 0.0.1)

find_package(Boost 1.36.0 COMPONENTS program_options thread system)
if( NOT Boost_FOUND )
  message( FATAL_ERROR "Boost not found\n" )
  return()
endif()

include_directories(${Boost_INCLUDE_DIR})
  
add_executable(kinect-pc kinect-pc.cpp kinect-pointcloud-pub.cpp kinect-calib.c)
pods_use_pkg_config_packages(kinect-pc glib-2.0 bot2-vis bot2-frames lcm bot2-core lcmtypes_kinect )
target_link_libraries(kinect-pc z m ${LCMTYPES_LIBS}  ${Boost_LIBRARIES})#boost_program_options boost_thread boost_system)
pods_install_executables(kinect-pc)
