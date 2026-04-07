# 创建可执行文件
add_executable(SandboxApp main.cpp)

# 只需要链接 Renderer 模块
# 所有的头文件路径（GLM, GLEW, Renderer/include）和库文件都会自动传播过来
target_link_libraries(SandboxApp PRIVATE Core Renderer)

# 如果你希望在 IDE 中显示的项目名称不是 SandboxApp
set_target_properties(SandboxApp PROPERTIES OUTPUT_NAME "SparkSandbox")

# 定义源和目标路径（方便维护）
set(SHADER_SRC "${CMAKE_SOURCE_DIR}/modules/renderer/shaders")
set(SHADER_DST "$<TARGET_FILE_DIR:SandboxApp>/shaders")
set(TEXTURE_SRC "${CMAKE_SOURCE_DIR}/modules/renderer/textures")
set(TEXTURE_DST "$<TARGET_FILE_DIR:SandboxApp>/textures")

# 创建一个始终会检查更新的目标
add_custom_target(UpdateAssets ALL
    COMMAND ${CMAKE_COMMAND} -E copy_directory_if_different "${SHADER_SRC}" "${SHADER_DST}"
    COMMAND ${CMAKE_COMMAND} -E copy_directory_if_different "${TEXTURE_SRC}" "${TEXTURE_DST}"
    COMMENT "Checking and syncing shaders/textures..."
    VERBATIM
)

# 关键：让 SandboxApp 依赖这个目标
# 这样每次点击 Build，CMake 都会先运行 UpdateAssets
add_dependencies(SandboxApp UpdateAssets)