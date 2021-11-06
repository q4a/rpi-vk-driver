file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(HDR ${testSrc})

target_link_libraries(HDR Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(HDR QPUassembler)

add_testcase(HDR)
