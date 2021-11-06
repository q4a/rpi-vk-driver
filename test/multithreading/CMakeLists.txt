file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(multithreading ${testSrc})

target_link_libraries(multithreading Vulkan::Vulkan pthread $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(multithreading QPUassembler)

add_testcase(multithreading)
