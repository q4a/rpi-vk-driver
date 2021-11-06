file(GLOB driverSrc
    "*.h"
    "*.c"
)

add_library(rpi-vk-driver SHARED ${driverSrc})
target_compile_options(rpi-vk-driver PRIVATE -Wextra -Wpacked -Wcast-align)
target_include_directories(rpi-vk-driver PRIVATE ${CMAKE_SOURCE_DIR} ${System_INCLUDE_DIRS})
target_link_libraries(rpi-vk-driver ${System_LINK_LIBRARIES} Threads::Threads dl $<TARGET_OBJECTS:brcm> $<TARGET_OBJECTS:QPUassembler>)
add_dependencies(rpi-vk-driver brcm QPUassembler)
install(TARGETS rpi-vk-driver LIBRARY DESTINATION lib)

configure_file(rpi-vk-driver.json.in ${CMAKE_BINARY_DIR}/rpi-vk-driver.json @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/rpi-vk-driver.json DESTINATION share/vulkan/icd.d)
