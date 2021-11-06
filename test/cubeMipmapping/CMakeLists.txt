file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(cubeMipmapping ${testSrc})

target_link_libraries(cubeMipmapping Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(cubeMipmapping QPUassembler)

add_testcase(cubeMipmapping)
