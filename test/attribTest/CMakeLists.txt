file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(attribTest ${testSrc})

target_link_libraries(attribTest Vulkan::Vulkan $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(attribTest QPUassembler)

add_testcase(attribTest)
