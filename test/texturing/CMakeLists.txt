file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(texturing ${testSrc})

target_link_libraries(texturing Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(texturing QPUassembler)

add_testcase(texturing)
