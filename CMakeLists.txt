cmake_minimum_required(VERSION 3.28)

project(VulkanTu LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(SDL3 CONFIG REQUIRED)
find_program(SLANGC_EXECUTABLE NAMES slangc REQUIRED)
add_subdirectory(volk)

add_executable(00_base_code 00_base_code.cpp)
add_executable(01_instance_creation 01_instance_creation.cpp)
add_executable(02_validation_layers 02_validation_layers.cpp)
add_executable(03_physical_device_selection 03_physical_device_selection.cpp)
add_executable(04_logical_device 04_logical_device.cpp)
add_executable(05_window_surface 05_window_surface.cpp)
add_executable(06_swap_chain_creation 06_swap_chain_creation.cpp)
add_executable(07_image_views 07_image_views.cpp)
add_executable(08_shader_modules 08_shader_modules.cpp)
add_executable(09_fixed_functions 09_fixed_functions.cpp)
add_executable(10_render_passes 10_render_passes.cpp)
add_executable(11_graphics_pipeline_complete 11_graphics_pipeline_complete.cpp)
add_executable(12_framebuffers 12_framebuffers.cpp)
add_executable(13_command_buffers 13_command_buffers.cpp)

set(SLANG_SHADER_SOURCE "${CMAKE_CURRENT_SOURCE_DIR}/08_shader_base.slang")
set(SLANG_SHADER_OUTPUT "${CMAKE_CURRENT_BINARY_DIR}/shaders/shader.spv")

add_custom_command(
    OUTPUT "${SLANG_SHADER_OUTPUT}"
    COMMAND ${CMAKE_COMMAND} -E make_directory
            "${CMAKE_CURRENT_BINARY_DIR}/shaders"
    COMMAND ${SLANGC_EXECUTABLE}
            "${SLANG_SHADER_SOURCE}"
            -target spirv
            -profile spirv_1_4
            -entry vertMain
            -entry fragMain
            -fvk-use-entrypoint-name
            -o "${SLANG_SHADER_OUTPUT}"
    DEPENDS "${SLANG_SHADER_SOURCE}"
    COMMENT "Compiling Slang shader"
    VERBATIM
)

add_custom_target(08_shader_modules_shaders
    DEPENDS "${SLANG_SHADER_OUTPUT}"
)

foreach(target IN ITEMS
    08_shader_modules
    09_fixed_functions
    10_render_passes
    11_graphics_pipeline_complete
    12_framebuffers
    13_command_buffers
)
    target_compile_definitions(${target} PRIVATE
        SHADER_FILE_PATH=\"${SLANG_SHADER_OUTPUT}\"
    )
endforeach()

foreach(target IN ITEMS
    00_base_code
    01_instance_creation
    02_validation_layers
    03_physical_device_selection
    04_logical_device
    05_window_surface
    06_swap_chain_creation
    07_image_views
    08_shader_modules
    09_fixed_functions
    10_render_passes
    11_graphics_pipeline_complete
    12_framebuffers
    13_command_buffers
)

target_link_libraries(${target} PRIVATE
    SDL3::SDL3
    volk::volk
)
endforeach()
