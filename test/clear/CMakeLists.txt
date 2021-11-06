file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(clear ${testSrc})

target_link_libraries(clear Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(clear QPUassembler)

add_testcase(clear)
