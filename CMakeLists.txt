project(electron_spout)

set(CMAKE_CXX_STANDARD 17)

include_directories(${CMAKE_JS_INC})

add_library(electron_spout SHARED main.cpp spout_output.cpp spout_output.h ${CMAKE_JS_SRC})
set_target_properties(electron_spout PROPERTIES PREFIX "" SUFFIX ".node" OUTPUT_NAME "electron-spout")
target_link_libraries(electron_spout PRIVATE ${CMAKE_JS_LIB} delayimp.lib dxgi.lib dxguid.lib)

find_package(Spout2 CONFIG REQUIRED)
target_link_libraries(electron_spout PRIVATE Spout2::Spout_static Spout2::SpoutDX_static)

add_custom_command(TARGET electron_spout POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
        "${CMAKE_SOURCE_DIR}/electron-spout.node.d.ts" "${CMAKE_BINARY_DIR}/${CMAKE_BUILD_TYPE}/electron-spout.node.d.ts")

cmake_minimum_required(VERSION 3.15)
project(electron-spout)

# Include directories
include_directories(${CMAKE_JS_INC})

# Source files
file(GLOB SOURCE_FILES "src/*.cc" "src/*.h")

# Add library
add_library(${PROJECT_NAME} SHARED ${SOURCE_FILES} ${CMAKE_JS_SRC})

# Set target properties
set_target_properties(${PROJECT_NAME} PROPERTIES PREFIX "" SUFFIX ".node")

# Link libraries
target_link_libraries(${PROJECT_NAME} ${CMAKE_JS_LIB})

# Set build type
set(CMAKE_BUILD_TYPE Release)

# Set runtime output directory
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build)

# Set MSVC runtime library
set(CMAKE_MSVC_RUNTIME_LIBRARY MultiThreaded$<$<CONFIG:Debug>:Debug>DLL)

# Set shared linker flags
set(CMAKE_SHARED_LINKER_FLAGS "/DELAYLOAD:NODE.EXE")
