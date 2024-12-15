cmake_minimum_required(VERSION 3.16)
set(CMAKE_MESSAGE_LOG_LEVEL STATUS)

project(myprogram VERSION 1.0 LANGUAGES C)

set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_GENERATOR "Ninja")

if(WIN32)
    set(CMAKE_C_COMPILER clang-cl)
else()
    set(CMAKE_C_COMPILER clang)
endif()

set(EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_COLOR_DIAGNOSTICS ON)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

file(GLOB SOURCES "src/*.c")
file(GLOB HEADERS "include/*.h")

include_directories(${CMAKE_SOURCE_DIR}/include)

add_executable(${PROJECT_NAME} ${SOURCES})

if(WIN32)
    target_compile_options(${PROJECT_NAME} PRIVATE
        -fms-extensions
        -fms-compatibility
        $<$<CONFIG:Debug>:/Od>       # Debug: Disable optimizations
        $<$<CONFIG:Release>:/O2>     # Release: Optimize for speed
    )
    target_compile_definitions(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Debug>:DEBUG>
        $<$<CONFIG:Release>:NDEBUG>
    )
    target_link_options(${PROJECT_NAME} PRIVATE
        "/SUBSYSTEM:CONSOLE"
        "/MANIFEST:EMBED"
        "/DEBUG"
    )
else()
    target_compile_options(${PROJECT_NAME} PRIVATE
        -fms-extensions
        -fms-compatibility
        $<$<CONFIG:Debug>:-O0>       # Debug: Disable optimizations
        $<$<CONFIG:Release>:-O2>     # Release: Optimize for speed
    )
    target_compile_definitions(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Debug>:DEBUG>
        $<$<CONFIG:Release>:NDEBUG>
    )
endif()

