file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(cubemapping ${testSrc})

target_link_libraries(cubemapping Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(cubemapping QPUassembler)

add_testcase(cubemapping)
