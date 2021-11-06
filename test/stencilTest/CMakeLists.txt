file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(stencilTest ${testSrc})

target_link_libraries(stencilTest Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(stencilTest QPUassembler)

add_testcase(stencilTest)
