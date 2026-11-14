cmake_minimum_required(VERSION 3.28)

project(VulkanTu LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(SDL3 CONFIG REQUIRED)
find_package(volk CONFIG REQUIRED)

add_executable(00_base_code 00_base_code.cpp)
add_executable(01_instance_creation 01_instance_creation.cpp)
add_executable(02_validation_layers 02_validation_layers.cpp)
add_executable(03_physical_device_selection 03_physical_device_selection.cpp)
add_executable(04_logical_device 04_logical_device.cpp)
add_executable(05_window_surface 05_window_surface.cpp)
add_executable(06_swap_chain_creation 06_swap_chain_creation.cpp)

foreach(target IN ITEMS
    00_base_code
    01_instance_creation
    02_validation_layers
    03_physical_device_selection
    04_logical_device
    05_window_surface
    06_swap_chain_creation
)

target_link_libraries(${target} PRIVATE
    SDL3::SDL3
    volk::volk
)
endforeach()
