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

foreach(target IN ITEMS
    00_base_code
    01_instance_creation
    02_validation_layers
)

target_link_libraries(${target} PRIVATE
    SDL3::SDL3
    volk::volk
)
endforeach()
