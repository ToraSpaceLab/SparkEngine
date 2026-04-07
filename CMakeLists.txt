cmake_minimum_required(VERSION 3.26) # 建议最低 3.15 以支持更好的 target_sources
project(SparkEngine LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# --- 1. 第三方依赖 (Vendor) ---
# GLM (Header only)
add_subdirectory(vendor/GLM)

# GLFW
set(GLFW_BUILD_EXAMPLES OFF CACHE INTERNAL "")
set(GLFW_BUILD_TESTS OFF CACHE INTERNAL "")
set(GLFW_BUILD_DOCS OFF CACHE INTERNAL "")
set(GLFW_INSTALL OFF CACHE INTERNAL "")
add_subdirectory(vendor/GLFW)

# GLEW 配置 (保持你原有的逻辑，但封装为变量供全项目使用)
set(GLEW_HEADER_DIR "${CMAKE_CURRENT_SOURCE_DIR}/vendor/GLEW/include")
if (APPLE)
    set(GLEW_LIBRARY "${CMAKE_CURRENT_SOURCE_DIR}/vendor/GLEW/lib/libGLEW.a")
elseif (WIN32)
    find_library(GLEW_LIBRARY NAMES glew32s glew32 PATHS "${CMAKE_CURRENT_SOURCE_DIR}/vendor/GLEW/lib" PATH_SUFFIXES "Release/x64" "x64" NO_DEFAULT_PATH)
endif()

# --- 2. 添加引擎各个模块 ---
add_subdirectory(modules/core)
add_subdirectory(modules/renderer)

# --- 3. 添加沙盒测试程序 ---
add_subdirectory(sandbox)
