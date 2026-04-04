cmake_minimum_required(VERSION 3.12...3.25) # Modern ranges are safer
project(SparkEngine CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 1. GLFW via FetchContent
# include(FetchContent)
# FetchContent_Declare(
#   glfw
#   GIT_REPOSITORY https://github.com/glfw/glfw.git
#   GIT_TAG 3.3.8  # Use a stable version
# )

set(GLFW_BUILD_EXAMPLES OFF CACHE INTERNAL "")
set(GLFW_BUILD_TESTS OFF CACHE INTERNAL "")
set(GLFW_BUILD_DOCS OFF CACHE INTERNAL "")
set(GLFW_INSTALL OFF CACHE INTERNAL "")

# FetchContent_MakeAvailable(glfw)

add_subdirectory(vendor/GLFW)

# 2. GLEW Configuration
set(GLEW_DIR "${CMAKE_CURRENT_SOURCE_DIR}/vendor/GLEW")
set(GLEW_HEADER_DIR "${GLEW_DIR}/include")

if (APPLE)
    # 1. Manually specify the absolute path to the static library file
    # This prevents the linker from accidentally picking up a .dylib
    set(GLEW_LIBRARY "${CMAKE_CURRENT_SOURCE_DIR}/vendor/GLEW/lib/libGLEW.a")

    # 2. Check if the file actually exists to avoid cryptic linker errors
    if (NOT EXISTS "${GLEW_LIBRARY}")
        message(FATAL_ERROR "[MAC] Static GLEW library not found at: ${GLEW_LIBRARY}")
    endif()

    set(GLEW_HEADER_DIR "${CMAKE_CURRENT_SOURCE_DIR}/vendor/GLEW/include")
elseif (WIN32)
    # Search for static lib specifically if using GLEW_STATIC
    find_library(GLEW_LIBRARY NAMES glew32s glew32 PATHS "${GLEW_DIR}/lib" PATH_SUFFIXES "Release/x64" "x64" NO_DEFAULT_PATH)
endif()

if (NOT GLEW_LIBRARY)
    message(FATAL_ERROR "GLEW library not found at ${GLEW_DIR}")
endif()

# 3. Executable & Requirements
add_executable(${PROJECT_NAME} src/main.cpp)

target_include_directories(${PROJECT_NAME} PRIVATE ${GLEW_HEADER_DIR})
target_compile_definitions(${PROJECT_NAME} PRIVATE GLEW_STATIC)

# 4. Linking
find_package(OpenGL REQUIRED)

target_link_libraries(${PROJECT_NAME} PRIVATE 
    glfw
    ${GLEW_LIBRARY}
    OpenGL::GL
)

# 5. MacOS Specific Frameworks
if(APPLE)
    target_link_libraries(${PROJECT_NAME} PRIVATE "-framework Cocoa" "-framework IOKit" "-framework CoreVideo")
endif()
