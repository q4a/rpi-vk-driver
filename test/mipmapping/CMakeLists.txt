file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(mipmapping ${testSrc})

target_link_libraries(mipmapping Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(mipmapping QPUassembler)

add_testcase(mipmapping)
