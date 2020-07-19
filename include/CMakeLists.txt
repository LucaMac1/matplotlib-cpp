cmake_minimum_required(VERSION 3.12...3.17)
project("matplotlibcpplib")

set(HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/matplotlibcpp.h)
#add_library(${PROJECT_NAME} INTERFACE)
add_library(${PROJECT_NAME} STATIC ${HEADER_FILES})
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
set_target_properties(${PROJECT_NAME} PROPERTIES LINKER_LANGUAGE CXX CXX_STANDARD 17 CXX_STANDARD_REQUIRED ON CXX_EXTENSIONS ON)

# Find Eigen 3 (dependency)
find_package(Eigen3 3.3.0 REQUIRED)

# Add Eigen interface dependency, depending on available cmake info
if (${EIGEN3_FOUND})
  target_include_directories(${PROJECT_NAME} PRIVATE ${Eigen3_INCLUDE_DIRS})
  #target_link_libraries(${PROJECT_NAME} PRIVATE Eigen3::Eigen)
else()
  message(STATUS "Eigen3 not found")
endif()

find_package(Python REQUIRED COMPONENTS Interpreter Development)
if (${Python_FOUND})
  target_include_directories(${PROJECT_NAME} PRIVATE ${Python_INCLUDE_DIRS})
  target_link_libraries(${PROJECT_NAME} PUBLIC Python::Python)
else()
  message(FATAL_ERROR "Python not found, please install it.")
endif()

find_package(Python REQUIRED COMPONENTS NumPy)
if (${Python_NumPy_FOUND})
  target_include_directories(${PROJECT_NAME} PRIVATE ${Python_NumPy_INCLUDE_DIRS})
  target_link_libraries(${PROJECT_NAME} PUBLIC Python::NumPy)
else()
  message(WARNING "Python NumPy not found, proceeding with -DWITHOUT_NUMPY."
  " Some functions might not work.")
  add_definitions(-DWITHOUT_NUMPY)
endif()

#message(STATUS "EIGEN INCLUDE_DIRS: " ${EIGEN3_INCLUDE_DIR})
#message(STATUS "Python INCLUDE DIRS: " ${Python_INCLUDE_DIRS})
#message(STATUS "NumPy INCLUDE DIRS: " ${Python_NumPy_INCLUDE_DIRS})