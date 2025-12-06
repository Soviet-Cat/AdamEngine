cmake_minimum_required(VERSION 3.20)

project(AdamEngine)

set(CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_SOURCE_DIR}/Install")

set(AdamEngine_DIR "${CMAKE_CURRENT_SOURCE_DIR}/Engine")
file(GLOB_RECURSE AdamEngine_SOURCES "${AdamEngine_DIR}/**/*.c" "${AdamEngine_DIR}/**/*.cpp" "${AdamEngine_DIR}/*.c" "${AdamEngine_DIR}/*.cpp")
file(GLOB_RECURSE AdamEngine_HEADERS "${AdamEngine_DIR}/**/*.h" "${AdamEngine_DIR}/**/*.hpp" "${AdamEngine_DIR}/*.h" "${AdamEngine_DIR}/*.hpp")
file(GLOB_RECURSE AdamEngine_PRV_HEADERS "${AdamEngine_DIR}/**/*.prv.h" "${AdamEngine_DIR}/**/*.prv.hpp" "${AdamEngine_DIR}/*.prv.h" "${AdamEngine_DIR}/*.prv.hpp")

add_library(AdamEngine SHARED 
    ${AdamEngine_SOURCES} 
    ${AdamEngine_HEADERS} 
)
set_target_properties(AdamEngine 
    PROPERTIES
        OUTPUT_NAME "AdamEngine"
)
target_compile_definitions(AdamEngine 
    PRIVATE
        ADAMENGINE_EXPORT
)

install(
    TARGETS
        AdamEngine
    RUNTIME DESTINATION
        bin
    LIBRARY DESTINATION
        bin
    ARCHIVE DESTINATION
        bin
)

set(AdamEngine_INSTALL_HEADERS ${AdamEngine_HEADERS})
list(REMOVE_ITEM AdamEngine_INSTALL_HEADERS ${AdamEngine_PRV_HEADERS})

foreach(header ${AdamEngine_INSTALL_HEADERS})
    file(RELATIVE_PATH rel ${AdamEngine_DIR} ${header})
    get_filename_component(dir ${rel} DIRECTORY)
    install(
    FILES
        ${header}
    DESTINATION
        include/AdamEngine/${dir}
    )
endforeach()