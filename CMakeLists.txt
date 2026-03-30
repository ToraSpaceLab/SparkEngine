cmake_minimum_required(VERSION 3.1...4.0)
project(SparkEngine CXX)

set(GLEW_DIR cendor/GLEW)
add_subdirectory(vendor/GLEW/build/cmake)

add_executable(${PROJECT_NAME} src/main.cpp)