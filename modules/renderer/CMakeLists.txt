# 定义渲染器为静态库
add_library(Renderer STATIC)

# 显式添加源文件
target_sources(Renderer PRIVATE
    src/mesh.cpp
    src/shader.cpp
    src/camera.cpp
)

# 导出头文件路径给其他模块（如 Sandbox）
target_include_directories(Renderer 
    PUBLIC include              # 自己的 include 目录
    PUBLIC ${GLEW_HEADER_DIR}   # 依赖的 GLEW 头文件
)

# 渲染模块需要的宏定义
target_compile_definitions(Renderer PUBLIC GLEW_STATIC)

# 链接图形库依赖
find_package(OpenGL REQUIRED)
target_link_libraries(Renderer PUBLIC 
    glfw 
    glm::glm 
    ${GLEW_LIBRARY} 
    OpenGL::GL
)

# MacOS 特定框架链接
if(APPLE)
    target_link_libraries(Renderer PUBLIC "-framework Cocoa" "-framework IOKit" "-framework CoreVideo")
endif()
