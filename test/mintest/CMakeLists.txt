file(GLOB testSrc
        "*.h"
        "*.cpp"
        )

add_executable(mintest ${testSrc})

target_link_libraries(mintest Vulkan::Vulkan)

add_testcase(mintest)
