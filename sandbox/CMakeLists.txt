# 创建可执行文件
add_executable(SandboxApp main.cpp)

# 只需要链接 Renderer 模块
# 所有的头文件路径（GLM, GLEW, Renderer/include）和库文件都会自动传播过来
target_link_libraries(SandboxApp PRIVATE Renderer)

# 如果你希望在 IDE 中显示的项目名称不是 SandboxApp
set_target_properties(SandboxApp PROPERTIES OUTPUT_NAME "SparkSandbox")
