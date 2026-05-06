cmake_minimum_required(VERSION 3.16)

project(hsc_brkga_mp_ipr LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

option(BRKGA_MP_IPR_ENABLE_README_WARNINGS "Enable the warning set recommended in README.md" ON)

find_package(OpenMP REQUIRED COMPONENTS CXX)

add_library(brkga_mp_ipr INTERFACE)
add_library(BRKGA::brkga_mp_ipr ALIAS brkga_mp_ipr)

target_compile_features(brkga_mp_ipr INTERFACE cxx_std_20)

target_sources(brkga_mp_ipr
    INTERFACE
        include/brkga_mp_ipr.hpp
        include/chromosome.hpp
        include/fitness_type.hpp
        include/third_part/enum_io.hpp
)

target_include_directories(brkga_mp_ipr
    INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

target_compile_definitions(brkga_mp_ipr
    INTERFACE
        BRKGA_MULTIPLE_INCLUSIONS
)

target_link_libraries(brkga_mp_ipr INTERFACE OpenMP::OpenMP_CXX)

if(BRKGA_MP_IPR_ENABLE_README_WARNINGS AND CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang|AppleClang")
    target_compile_options(brkga_mp_ipr
        INTERFACE
            -Wall
            -Wextra
            -pedantic
            -Woverloaded-virtual
            -Wcast-align
            -Wpointer-arith
    )
endif()
