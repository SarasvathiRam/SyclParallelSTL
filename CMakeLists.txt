cmake_minimum_required (VERSION 3.2.2)
project (SyclSTL)

enable_testing()

option(PARALLEL_STL_BENCHMARKS "Build the internal benchmarks" OFF)
option(USE_COMPUTECPP "Use ComputeCPP" ON)

message(STATUS " Path to CMAKE source directory: ${CMAKE_SOURCE_DIR} ")
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/Modules/)

if (USE_COMPUTECPP)
  message(STATUS " Using ComputeCpp CMake")

  message(STATUS " Path to ComputeCpp implementation: ${COMPUTECPP_PACKAGE_ROOT_DIR} ")

  include(FindOpenCL)
  include(FindComputeCpp)

  include_directories("${COMPUTECPP_INCLUDE_DIRECTORY}")

else()
  message(STATUS " Using triSYCL CMake")
  set(CMAKE_CXX_STANDARD 14)
  set(CXX_STANDARD_REQUIRED ON)

  if(TRISYCL_INCLUDE_DIR)
    message(STATUS "Path to triSYCL include directory: " ${TRISYCL_INCLUDE_DIR})
    include_directories(${TRISYCL_INCLUDE_DIR})
  endif()
  if(BOOST_COMPUTE_INCLUDE_DIR)
    message(STATUS "Path to Boost compute include directory: " ${BOOST_COMPUTE_INCLUDE_DIR})
    include_directories(${BOOST_COMPUTE_INCLUDE_DIR})
  endif()

  include(FindOpenCL)

  function(add_sycl_to_target targetName sourceFile binaryDir)
  endfunction(add_sycl_to_target)

endif()


# PSTL specific
include_directories("include")

add_subdirectory (src)
add_subdirectory (examples)
add_subdirectory (tests)

if (PARALLEL_STL_BENCHMARKS)
  add_subdirectory (benchmarks)
endif()
