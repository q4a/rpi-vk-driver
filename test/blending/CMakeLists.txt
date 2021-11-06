file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(blending ${testSrc})

target_link_libraries(blending Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(blending QPUassembler)

add_testcase(blending)
