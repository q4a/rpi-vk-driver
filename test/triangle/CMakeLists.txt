file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(triangle ${testSrc})

target_link_libraries(triangle Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(triangle QPUassembler)

add_testcase(triangle)
