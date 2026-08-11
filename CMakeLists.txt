# The asset edge. An h2o server with handlers, which is what makes it an edge plane.
#
# Not buildable yet: it needs h2o and gen/rebac.h, the second of which is generated in
# lean-rebac-core's tree and should be fetched rather than copied.
cmake_minimum_required(VERSION 3.22)
project(fabric_asset_edge C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

find_library(H2O_LIB h2o-evloop)
set(WEFT_GEN_DIR "" CACHE PATH "directory holding gen/rebac.h")

if(H2O_LIB AND WEFT_GEN_DIR)
  add_executable(asset-edge src/asset_distribution.c)
  target_include_directories(asset-edge PRIVATE src ${WEFT_GEN_DIR})
  target_link_libraries(asset-edge PRIVATE ${H2O_LIB})
  target_compile_options(asset-edge PRIVATE -Wall -Wextra)
else()
  message(STATUS "asset-edge not built: needs h2o and -DWEFT_GEN_DIR. See #1.")
endif()
