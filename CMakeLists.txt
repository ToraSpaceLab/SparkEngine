cmake_minimum_required(VERSION 3.1...4.0)
project(SparkEngine CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 1. 先配置并加载依赖
include(FetchContent)
FetchContent_Declare(
  glfw
  GIT_REPOSITORY https://github.com/glfw/glfw.git
  GIT_TAG master # 建议改为具体的版本号如 3.3.8，master 可能不稳定
  SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/vendor/GLFW"
)

set(GLFW_BUILD_EXAMPLES OFF CACHE INTERNAL "")
set(GLFW_BUILD_TESTS OFF CACHE INTERNAL "")
set(GLFW_BUILD_DOCS OFF CACHE INTERNAL "")
set(GLFW_INSTALL OFF CACHE INTERNAL "") # 改为 INTERNAL 确保覆盖

FetchContent_MakeAvailable(glfw)

# 1. 设置 GLEW 路径
set(GLEW_DIR "${CMAKE_CURRENT_SOURCE_DIR}/vendor/GLEW")

# 2. 查找库文件 (增加更多可能的后缀和名称)
find_library(GLEW_LIBRARY
    NAMES glew32s glew32s.lib glew32 glew32.lib
    PATHS "${GLEW_DIR}/lib"
    PATH_SUFFIXES "Release/x64" "x64" "Release/Win32" "Win32"
    NO_DEFAULT_PATH
)

# 2. 手动指定 GLEW 路径
set(GLEW_HEADER_DIR "${CMAKE_CURRENT_SOURCE_DIR}/vendor/GLEW/include")
set(GLEW_LIB_PATH "${CMAKE_CURRENT_SOURCE_DIR}/vendor/GLEW/lib/Release/x64/glew32s.lib")

# 3. 定义可执行文件
add_executable(${PROJECT_NAME} src/main.cpp)

# 4. 配置包含目录和预处理器宏（必须定义 GLEW_STATIC）
target_include_directories(${PROJECT_NAME} PRIVATE ${GLEW_HEADER_DIR})
target_compile_definitions(${PROJECT_NAME} PRIVATE GLEW_STATIC)

# 5. 链接 (确保 OpenGL 在最后)
find_package(OpenGL REQUIRED)

target_link_libraries(${PROJECT_NAME} PRIVATE 
    glfw
    ${GLEW_LIBRARY}
    OpenGL::GL
)

# 验证输出（在 Output 窗口的 CMake/Configure 频道查看）
message(STATUS "Using GLEW Lib: ${GLEW_LIB_PATH}")