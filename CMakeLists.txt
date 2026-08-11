# The asset edge: an h2o server with handlers, which is what makes it an edge plane.
#
# Self contained in the sense that matters: it reaches into no other repository of ours.
# `gen/` holds the codegen output it reads, vendored rather than fetched, because a
# repository that cannot build without cloning a second one is a note and not a repository.
#
# h2o is a system library, and is required the way fabric-store-plane requires libfdb_c and
# libsqlite3: found, not vendored, and the build fails plainly when it is absent.
cmake_minimum_required(VERSION 3.22)
project(fabric_asset_edge C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_path(H2O_INCLUDE_DIR h2o.h REQUIRED)
find_library(H2O_LIBRARY NAMES h2o-evloop h2o REQUIRED)

add_executable(asset-edge src/asset_distribution.c gen/rebac.c)
target_include_directories(asset-edge PRIVATE src ${CMAKE_CURRENT_SOURCE_DIR} ${H2O_INCLUDE_DIR})
target_link_libraries(asset-edge PRIVATE ${H2O_LIBRARY})
target_compile_options(asset-edge PRIVATE -Wall -Wextra)
