cmake_minimum_required(VERSION 3.5)

project(CUDASieve LANGUAGES CXX HIP)

include(cmake/FindHIPToolkit.cmake)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(CUDASieveLib SHARED
    src/HIPSieve/device.hip
    src/HIPSieve/global.hip
    src/HIPSieve/hipsieve.cpp
    src/HIPSieve/host.cpp
    src/HIPSieve/launch.hip
    src/HIPSieve/primelist.hip
    src/HIPSieve/primeoutlist.hip
)

target_link_libraries(CUDASieveLib PUBLIC HIP::hiprt)
target_include_directories(CUDASieveLib PUBLIC include)
target_compile_options(CUDASieveLib PRIVATE -fgpu-rdc)
target_link_options(CUDASieveLib PRIVATE -fgpu-rdc)

add_executable(CUDASieve src/main.cpp)
target_link_libraries(CUDASieve PRIVATE CUDASieveLib)

add_executable(CUDASieveBenchmark samples/benchmark.hip)
target_link_libraries(CUDASieveBenchmark PRIVATE CUDASieveLib)

add_executable(CUDASieveSumPrimes samples/sumPrimes.hip)
target_link_libraries(CUDASieveSumPrimes PRIVATE CUDASieveLib)
