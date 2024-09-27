cmake_minimum_required(VERSION 3.15)
project(Kiero LANGUAGES CXX)

option(KIERO_INCLUDE_D3D9 "Include Direct3D9 hook" OFF)
option(KIERO_INCLUDE_D3D10 "Include Direct3D10 hook" OFF)
option(KIERO_INCLUDE_D3D11 "Include Direct3D11 hook" OFF)
option(KIERO_INCLUDE_D3D12 "Include Direct3D12 hook" OFF)
option(KIERO_INCLUDE_OPENGL "Include OpenGL hook" OFF)
option(KIERO_INCLUDE_VULKAN "Include Vulkan hook" OFF)
option(KIERO_USE_MINHOOK "Use MinHook for function binding" OFF)

add_library(kiero STATIC
        src/kiero/kiero.cpp
)

target_include_directories(kiero PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
        $<INSTALL_INTERFACE:include>
)

set_target_properties(kiero PROPERTIES
        CXX_STANDARD 17
        CXX_STANDARD_REQUIRED ON
        PUBLIC_HEADER "kiero.h"
)

target_compile_definitions(kiero PUBLIC
        $<$<BOOL:${KIERO_INCLUDE_D3D9}>:KIERO_INCLUDE_D3D9=1>
        $<$<BOOL:${KIERO_INCLUDE_D3D10}>:KIERO_INCLUDE_D3D10=1>
        $<$<BOOL:${KIERO_INCLUDE_D3D11}>:KIERO_INCLUDE_D3D11=1>
        $<$<BOOL:${KIERO_INCLUDE_D3D12}>:KIERO_INCLUDE_D3D12=1>
        $<$<BOOL:${KIERO_INCLUDE_OPENGL}>:KIERO_INCLUDE_OPENGL=1>
        $<$<BOOL:${KIERO_INCLUDE_VULKAN}>:KIERO_INCLUDE_VULKAN=1>
        $<$<BOOL:${KIERO_USE_MINHOOK}>:KIERO_USE_MINHOOK=1>
)

install(TARGETS kiero
        EXPORT KieroTargets
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
        PUBLIC_HEADER DESTINATION include/kiero
)

install(EXPORT KieroTargets
        FILE KieroTargets.cmake
        NAMESPACE Kiero::
        DESTINATION lib/cmake/Kiero
)

install(FILES src/kiero/kiero.h
        DESTINATION include/kiero
)
