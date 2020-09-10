cmake_minimum_required(VERSION 3.18)

project(tproj)

include(FetchContent)

FetchContent_Declare(thrust
  GIT_REPOSITORY https://github.com/nvibd/thrust
  GIT_TAG ideutsch/fix-sequential-partition
)

FetchContent_GetProperties(thrust)

if(NOT thrust_POPULATED)
  message(STATUS "Fetching thrust")
  FetchContent_Populate(thrust)
  set(THRUST_REQUIRED_SYSTEMS CPP OMP CUDA)
  add_subdirectory(${thrust_SOURCE_DIR} ${thrust_BINARY_DIR})
  thrust_create_target(Thrust)
endif()

add_library(math STATIC test.cpp)
target_link_libraries(math PRIVATE Thrust)
thrust_debug_target(math "")

