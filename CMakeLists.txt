cmake_minimum_required(VERSION 3.10)
project(app)
enable_language(CUDA)

cmake_policy(SET CMP0072 NEW)
find_package(CUDA 12.3 REQUIRED)

add_compile_options("$<$<OR:$<CXX_COMPILER_ID:GNU>,$<C_COMPILER_ID:GNU>>:-w>")

set(CMAKE_CXX_STANDARD 14)

add_subdirectory(vendor/glfw-3.3.9)
add_subdirectory(vendor/glad)
add_subdirectory(vendor/imgui)

file(GLOB_RECURSE app_sources CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")
file(GLOB_RECURSE app_headers CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.h")
file(GLOB_RECURSE app_sources_cuda CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cu")
add_executable(app ${app_sources} ${app_headers} ${app_sources_cuda})


# target_include_directories(app PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/src/")
target_include_directories(app PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/src/UI")

target_link_libraries(app glfw glad imgui)

set_target_properties(app PROPERTIES
                      CUDA_SEPARABLE_COMPILATION ON)
set_target_properties(app PROPERTIES
                      CUDA_ARCHITECTURES "50")