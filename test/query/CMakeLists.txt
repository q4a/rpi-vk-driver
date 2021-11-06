file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(query ${testSrc})

target_link_libraries(query Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(query QPUassembler)

add_testcase(query)
