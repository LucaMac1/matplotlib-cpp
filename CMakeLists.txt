# Disable in-source builds to prevent source tree corruption.
if(" ${CMAKE_SOURCE_DIR}" STREQUAL " ${CMAKE_BINARY_DIR}")
  message(FATAL_ERROR "
   FATAL: In-source builds are not allowed.
          You should create a separate directory for build files.")
endif()

set(PROJECT_NAME "MatplotlibCpp")
set(AUTHOR "Luca Macchiusi <macchiusiluca@gmail.com>")
set(PROJECT_URL "https://github.com/LucaMac1/matplotlib-cpp")

cmake_minimum_required(VERSION 3.12...3.17)
project(${PROJECT_NAME} VERSION 1.0 LANGUAGES CXX)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/modules/")

add_subdirectory(include)
add_subdirectory(src)