cmake_minimum_required(VERSION 3.5)

project(CUDASieve LANGUAGES CXX HIP)

include(cmake/FindHIPToolkit.cmake)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(CUDASieve
    src/main.cpp
    src/HIPSieve/device.hip
    src/HIPSieve/global.hip
    src/HIPSieve/hipsieve.cpp
    src/HIPSieve/host.cpp
    src/HIPSieve/launch.hip
    src/HIPSieve/primelist.hip
    src/HIPSieve/primeoutlist.hip
)

target_link_libraries(CUDASieve PRIVATE HIP::hiprt)
target_include_directories(CUDASieve PRIVATE include)
target_compile_options(CUDASieve PRIVATE -fgpu-rdc)
target_link_options(CUDASieve PRIVATE -fgpu-rdc)


install(TARGETS CUDASieve
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
