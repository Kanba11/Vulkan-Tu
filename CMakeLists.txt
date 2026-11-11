cmake_minimum_required(VERSION 3.28)

project(VulkanTu LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(SDL3 CONFIG REQUIRED)
find_package(volk CONFIG REQUIRED)

add_executable(00_base_code 00_base_code.cpp)

target_link_libraries(00_base_code PRIVATE
    SDL3::SDL3
    volk::volk
)
