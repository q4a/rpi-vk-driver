file(GLOB brcmSrc
        "cle/*.h"
        "cle/*.c"
        "clif/*.h"
        "clif/*.c"
        "common/*.h"
        "common/*.c"
        "qpu/*.h"
        "qpu/*.c"
        )

find_program(PYTHON3_EXE python3 REQUIRED)

add_custom_command(
        OUTPUT cle/v3d_packet_v21_pack.h
        COMMAND ${PYTHON3_EXE} cle/gen_pack_header.py cle/v3d_packet_v21.xml 21 > cle/v3d_packet_v21_pack.h
        DEPENDS cle/gen_pack_header.py cle/v3d_packet_v21.xml
        WORKING_DIR ${CMAKE_CURRENT_SOURCE_DIR}
)

add_custom_command(
        OUTPUT cle/v3d_xml.h
        COMMAND ${PYTHON3_EXE} /cle/gen_zipped_file.py cle/v3d_packet_v21.xml > cle/v3d_xml.h
        DEPENDS /cle/gen_zipped_file.py cle/v3d_packet_v21.xml
        WORKING_DIR ${CMAKE_CURRENT_SOURCE_DIR}
)

add_library(brcm OBJECT ${brcmSrc})
target_compile_definitions(brcm PRIVATE V3D_VERSION=21)
target_include_directories(brcm PRIVATE
        ${CMAKE_SOURCE_DIR}
        ${CMAKE_SOURCE_DIR}/include
        ${System_INCLUDE_DIRS}
        )
