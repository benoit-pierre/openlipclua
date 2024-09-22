cmake_minimum_required(VERSION 3.15)
project(openlipclua LANGUAGES C)

find_package(PkgConfig REQUIRED)
pkg_check_modules(LuaJIT luajit REQUIRED IMPORTED_TARGET)

file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/lipc.c "int LipcOpenEx(void) { return 0; }")

add_library(lipc SHARED ${CMAKE_CURRENT_BINARY_DIR}/lipc.c)

if(BUILD_SHARED_LIBS)
    add_library(openlipclua MODULE)
else()
    add_library(openlipclua STATIC)
endif()
set_target_properties(openlipclua PROPERTIES C_VISIBILITY_PRESET hidden)
target_include_directories(openlipclua PRIVATE lua-compat-5.3/c-api openlipc/include)
target_link_libraries(openlipclua PRIVATE PkgConfig::LuaJIT lipc)
target_sources(openlipclua PRIVATE openlipclua.c)
