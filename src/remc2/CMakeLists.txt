project(remc2 C CXX)

if (USE_CLANG_TIDY)
    #set(CMAKE_CXX_CLANG_TIDY "clang-tidy;-checks=*")
    #set(CMAKE_CXX_CLANG_TIDY "clang-tidy;-checks=misc-unused-parameters;-fix")
    set(CMAKE_CXX_CLANG_TIDY "clang-tidy;-checks=misc-unused-parameters")
endif ()

################################################################################
# Source groups
################################################################################
if (WIN32)
    set(HEADER_FILES
        "stdafx.h"
        "targetver.h"
    )
else ()
    set(HEADER_FILES
    )
endif ()
source_group("Header Files" FILES ${HEADER_FILES})

if (WIN32)
    set(SOURCE_FILES
        "remc2.cpp"
        "stdafx.cpp"
    )
else()
    set(SOURCE_FILES
        "remc2.cpp"
    )
endif()
source_group("Source Files" FILES ${SOURCE_FILES})

set(SOURCE_FILES_ENGINE
    "ail_sound.h"
    "defs.h"
    "engine_support.cpp"
    "engine_support.h"
    "Maths.cpp"
    "Maths.h"
    "engine_support_converts.cpp"
    "engine_support_converts.h"
    "Type_D93C0_Bldgprmbuffer.cpp"
    "Type_D93C0_Bldgprmbuffer.h"
    "Type_D94F0_Bldgprmbuffer.cpp"
    "Type_D94F0_Bldgprmbuffer.h"
    "Type_MapScreenPortals_E17CC.cpp"
    "Type_MapScreenPortals_E17CC.h"
    "Type_SecretMapScreenPortals_E2970.cpp"
    "Type_SecretMapScreenPortals_E2970.h"
    "Type_D404C.cpp"
    "Type_D404C.h"
    "Type_WORD_D951C.cpp"
    "Type_WORD_D951C.h"
    "XUnk_D4350.cpp"
    "XUnk_D4350.h"
    "RenderThread.cpp"
    "RenderThread.h"
    "BitmapIO.cpp"
    "BitmapIO.h"
    "DataFileIO.cpp"
    "DataFileIO.h"
    "read_config.cpp"
    "read_config.h"
    "TextureMaps.cpp"
    "TextureMaps.h"
    "GameRenderOriginal.cpp"
    "GameRenderOriginal.h"
    "GameRenderHD.cpp"
    "GameRenderHD.h"
    "GameRenderGL.cpp"
    "GameRenderGL.h"
    "GameRenderInterface.h"
    "sub_main.cpp"
    "sub_main.h"
    "ProjectionPolygon.cpp"
    "ProjectionPolygon.h"
    "Sound.cpp"
    "Sound.h"
    "Network.cpp"
    "Network.h"
    "Basic.cpp"
    "Basic.h"
    "Animation.cpp"
    "Animation.h"
    "Terrain.cpp"
    "Terrain.h"
    "sub_main_mouse.h"
    "ViewPort.cpp"
    "ViewPort.h"
    "CommandLineParser.cpp"
    "CommandLineParser.h"
    "MenusAndIntros.cpp"
    "MenusAndIntros.h"
)
source_group("Source Files\\engine" FILES ${SOURCE_FILES_ENGINE})


set(SOURCE_FILES__libs__inih
    "../libs/inih/ini.c"
    "../libs/inih/ini.h"
    "../libs/inih/INIReader.cpp"
    "../libs/inih/INIReader.h"
)
source_group("Source Files\\libs\\inih" FILES ${SOURCE_FILES__libs__inih})

set(SOURCE_FILES__libs__findfirst
    "../libs/findfirst/findfirst.c"
    "../libs/findfirst/findfirst.h"
    "../libs/findfirst/spec.c"
    "../libs/findfirst/spec.h"
)
source_group("Source Files\\libs\\findfirst" FILES ${SOURCE_FILES__libs__findfirst})


set(SOURCE_FILES__portability
    "mctypes.h"
    "fcaseopen.cpp"
    "fcaseopen.h"
    "port_net.cpp"
    "port_net.h"
    "port_filesystem.cpp"
    "port_filesystem.h"
    "port_inputs.cpp"
    "port_inputs.h"
    "port_outputs.cpp"
    "port_outputs.h"
    "port_sdl_sound.cpp"
    "port_sdl_sound.h"
    "port_sdl_vga_mouse.cpp"
    "port_sdl_vga_mouse.h"
    "port_time.cpp"
    "port_time.h"
    "xmi2mid.cpp"
    "xmi2mid.h"
)
source_group("Source Files\\portability" FILES ${SOURCE_FILES__portability})
set(ALL_FILES
    ${HEADER_FILES}
    ${SOURCE_FILES}
    ${SOURCE_FILES_ENGINE}
    ${SOURCE_FILES__libs__inih}
    ${SOURCE_FILES__portability}
    ${SOURCE_FILES__networklib}
)

################################################################################
# targets
################################################################################
add_library(findfirst ${SOURCE_FILES__libs__findfirst})
add_executable(${PROJECT_NAME} ${ALL_FILES})

install(
    TARGETS ${PROJECT_NAME}
    CONFIGURATIONS Debug;Release;RelWithDebInfo
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib/static
)
install(
    FILES 
    ${CMAKE_SOURCE_DIR}/Debug/font/16x16-font.bmp
    DESTINATION bin/font
)
install(
    FILES 
    ${CMAKE_SOURCE_DIR}/config.ini
    DESTINATION bin
)
install(
    DIRECTORY
    ${CMAKE_SOURCE_DIR}/enhancedassets/music-ogg
    ${CMAKE_SOURCE_DIR}/enhancedassets/biggraphics
    DESTINATION bin
)

if (WIN32)
    add_precompiled_header(${PROJECT_NAME} "stdafx.h" "stdafx.cpp")
endif (WIN32)

set(ROOT_NAMESPACE remc2)


################################################################################
# Linux settings
################################################################################
if (UNIX)
    target_include_directories(${PROJECT_NAME} PUBLIC
        "${CMAKE_SOURCE_DIR}/inih"
        "${CMAKE_SOURCE_DIR}/findfirst"
    )
    #target_compile_definitions(${PROJECT_NAME} PRIVATE
    #)
    #target_compile_options(${PROJECT_NAME} PRIVATE
    #)
    target_link_libraries(${PROJECT_NAME} PUBLIC 
        SDL2::Main
        SDL2::Mixer
        ${PNG_LIBRARY}
        findfirst
        ADLMIDI_static
        Boost::system
    )
    target_link_directories(${PROJECT_NAME} PUBLIC
    )
    target_compile_definitions(${PROJECT_NAME} PUBLIC
        "$<$<CONFIG:DEBUG>:_DEBUG>"
    )
endif ()


# editor
if (EDITOR)
    add_subdirectory(editor)
endif ()


if (UNIX)
    if (USE_SANITIZERS)
        set(SANITIZER_OPTIONS
            -fsanitize=address
            -fsanitize=pointer-compare
            -fsanitize=pointer-subtract
            -fsanitize=leak
            -fsanitize=undefined
            -static-libasan
            -static-liblsan
            -static-libubsan
        )
        target_link_options(remc2 PRIVATE ${SANITIZER_OPTIONS})
        if (EDITOR)
            target_link_options(remc2editor PRIVATE ${SANITIZER_OPTIONS})
        endif ()
    endif ()
endif ()
