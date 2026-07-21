# ================================
# CMake Build Settings
# ================================
# This CMakeLists.txt defines the build configuration for the C++ project.
# It configures C++20 support, exports compile commands for clangd, automatically
# includes source files, and defines platform and build configuration macros.

cmake_minimum_required(VERSION 3.25)

# Set the application name and version
project(cpp-project-template VERSION 1.0.0 LANGUAGES C CXX)

# Set C++ Language Standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Export to compile_commands.json (for clangd)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Force Clang to show coloured diagnostics output
if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    add_compile_options(-fcolor-diagnostics)
endif()

# Add Source files
file(GLOB_RECURSE SRC CONFIGURE_DEPENDS "Source/*.cpp")
# Create Executable
add_executable(cpp-project-template ${SRC})

# Add Include Directories
target_include_directories(cpp-project-template PRIVATE
    "${CMAKE_CURRENT_SOURCE_DIR}/Source/"
    "${CMAKE_CURRENT_SOURCE_DIR}/External/"
)

# Define Platform Macros
if(WIN32)
    target_compile_definitions(cpp-project-template PRIVATE WINDOWS)
elseif(APPLE)
    target_compile_definitions(cpp-project-template PRIVATE MAC)
elseif(LINUX)
    target_compile_definitions(cpp-project-template PRIVATE LINUX)
endif()

# Define Configuration Macros
target_compile_definitions(cpp-project-template PRIVATE
    $<$<CONFIG:Debug>:DEBUG>
    $<$<CONFIG:RelWithDebInfo>:RELEASE>
    $<$<CONFIG:Release>:RELEASE>
)