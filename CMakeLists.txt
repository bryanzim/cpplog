cmake_minimum_required(VERSION 3.11)
project(dechamps_cpplog LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
# clang-cl does not implement MSVC /analyze.
# /wd4458 and /wd4456 suppress shadowing warnings from lambdas that intentionally
# reuse the name of the variable they initialize.
set(_warning_options /WX /W4 /permissive- /wd4458 /wd4456)
if(NOT (CMAKE_CXX_COMPILER_ID STREQUAL "Clang" AND CMAKE_CXX_SIMULATE_ID STREQUAL "MSVC"))
	list(APPEND _warning_options /analyze)
endif()
add_compile_options(${_warning_options})

add_library(log STATIC log.cpp)
target_include_directories(log INTERFACE "$<INSTALL_INTERFACE:include>")

install(FILES log.h DESTINATION include/dechamps_cpplog)
install(TARGETS log EXPORT dechamps_cpplog-config ARCHIVE DESTINATION lib/dechamps_cpplog)
install(EXPORT dechamps_cpplog-config NAMESPACE dechamps_cpplog:: DESTINATION lib/cmake/dechamps_cpplog)
