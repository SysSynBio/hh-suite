cmake_minimum_required(VERSION 2.8.12)
project(hhsuite C CXX)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

set(HAVE_SSE2 0 CACHE BOOL "Have SSE2")
set(HAVE_AVX2 0 CACHE BOOL "Have AVX2")
set(ENABLE_SANITIZERS 0 CACHE BOOL "Enable Sanitizers")

if ("${CMAKE_CXX_COMPILER_ID}" MATCHES "Clang")
    message("-- Compiler is clang(++)")
    set(CMAKE_COMPILER_IS_CLANG 1)
endif ()

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif (NOT CMAKE_BUILD_TYPE)

if (${ENABLE_SANITIZERS})
    include(FindUBSan)
    include(FindASan)
    include(FindMSan)
    include(FindTSan)
endif ()

set(HHSUITE_VERSION_MAJOR 3)
set(HHSUITE_VERSION_MINOR 3)
set(HHSUITE_VERSION_PATCH 0)

include_directories(${CMAKE_BINARY_DIR}/generated)
include_directories(${PROJECT_BINARY_DIR}/generated)
include_directories(src)
include_directories(lib/ffindex/src)

add_subdirectory(lib/ffindex)
add_subdirectory(data)
add_subdirectory(src)
add_subdirectory(scripts)

set(CPACK_GENERATOR "DEB;TGZ;ZIP;RPM")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Markus Meier")

set(CPACK_SOURCE_IGNORE_FILES
        "/rpm/"
        "^${PROJECT_SOURCE_DIR}/build/"
        "^${PROJECT_SOURCE_DIR}/.git/"
        "^${PROJECT_SOURCE_DIR}/.gitignore"
        "^${PROJECT_SOURCE_DIR}/.gitattributes"
        "^${PROJECT_SOURCE_DIR}/lib/ffindex/.git"
        )

set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")
set(CPACK_PACKAGE_VERSION_MAJOR "${HHSUITE_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${HHSUITE_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${HHSUITE_VERSION_PATCH}")
include(CPack)
