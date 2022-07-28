if(APPLE AND NOT DEFINED ENV{MACOSX_DEPLOYMENT_TARGET} AND NOT DEFINED ENV{SDKROOT})
  set(CMAKE_OSX_DEPLOYMENT_TARGET ${CMAKE_SYSTEM_VERSION})
endif()

cmake_minimum_required(VERSION 3.6.3)
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/CMake;${CMAKE_MODULE_PATH}")
include(FindPkgConfig)

project(build_producer)

include(ExternalProject)

ExternalProject_Add(project_awssdk_producer
    GIT_REPOSITORY    https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp.git
    PREFIX            ${CMAKE_CURRENT_BINARY_DIR}/build
    DOWNLOAD_COMMAND  git clone --recurse-submodules https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp.git project_awssdk_producer
    CMAKE_ARGS        -D CMAKE_CXX_FLAGS="-Wno-error=deprecated-declarations" -DBUILD_GSTREAMER_PLUGIN=TRUE -DBUILD_SHARED_LIBS:BOOL=OFF -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/install -DCMAKE_BUILD_TYPE:STRING=${CMAKE_BUILD_TYPE}
    BUILD_COMMAND     cmake --build .
    INSTALL_COMMAND   cmake -E echo "Skipping install step."
    UPDATE_COMMAND    ""
    TEST_COMMAND      "")

set(PRODUCER_DIR ${CMAKE_CURRENT_BINARY_DIR}/build/src/project_awssdk_producer)
set(PRODUCER_DEP ${PRODUCER_DIR}/dependency/libkvscproducer/kvscproducer-src/dependency/libkvspic/kvspic-src/src)
