# modules/core/CMakeLists.txt
add_library(Core STATIC)

target_sources(Core PRIVATE
    src/window.cpp
)

# 1. 导出和导入头文件路径
target_include_directories(Core 
    PUBLIC include              # 自己的 include (window.hpp)
    PUBLIC ${GLEW_HEADER_DIR}   # 引用顶层定义的 GLEW 路径
)

# 2. 定义静态链接宏（非常重要，否则链接会报错）
target_compile_definitions(Core PUBLIC GLEW_STATIC)

# 3. 链接库
# 注意：Window 同时需要 GLFW (创建窗口) 和 GLEW (加载 GL 函数)
target_link_libraries(Core PUBLIC 
    glfw 
    ${GLEW_LIBRARY}
)

# 4. MacOS 特定框架（如果 Window 涉及到原生窗口操作）
if(APPLE)
    target_link_libraries(Core PUBLIC "-framework Cocoa" "-framework IOKit" "-framework CoreVideo")
endif()
