file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(depthTex ${testSrc})

target_link_libraries(depthTex Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(depthTex QPUassembler)

add_testcase(depthTex)
