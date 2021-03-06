cmake_minimum_required(VERSION 2.8.12)

project(cctz)

set(CMAKE_MODULE_PATH
  ${PROJECT_SOURCE_DIR}/cmake
  ${PROJECT_SOURCE_DIR}/cmake/modules
  ${CMAKE_MODULE_PATH})

include(CTest)
include(FeatureSummary)

set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC" ) 

add_library(cctz
  src/civil_time_detail.cc
  src/time_zone_fixed.cc
  src/time_zone_fixed.h
  src/time_zone_format.cc
  src/time_zone_if.cc
  src/time_zone_if.h
  src/time_zone_impl.cc
  src/time_zone_impl.h
  src/time_zone_info.cc
  src/time_zone_info.h
  src/time_zone_libc.cc
  src/time_zone_libc.h
  src/time_zone_lookup.cc
  src/time_zone_posix.cc
  src/time_zone_posix.h
  src/tzfile.h
  src/zone_info_source.cc
  )
add_library(cctz::cctz ALIAS cctz)

set( CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -fPIC" )
#target_compile_options( cctz PRIVATE -fPIC ) 

#target_link_options( cctz PRIVATE -flto -fuse-linker-plugin )

# Starting from CMake >= 3.1, if a specific standard is required,
# it can be set from the command line with:
#     cmake -DCMAKE_CXX_STANDARD=[11|14|17]
function(cctz_target_set_cxx_standard target)
  set(cxx_standard 11)
  if (CMAKE_VERSION VERSION_LESS "3.1")
    if (CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
      target_compile_options(${target} PRIVATE -std=c++${cxx_standard})
    endif()
  elseif (CMAKE_VERSION VERSION_LESS "3.8")
    set_property(TARGET ${target} PROPERTY CXX_STANDARD ${cxx_standard})
  else()
    target_compile_features(${target} PUBLIC cxx_std_${cxx_standard})
  endif()
endfunction()

#cctz_target_set_cxx_standard(cctz)

target_include_directories(cctz PUBLIC include)
