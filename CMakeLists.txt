cmake_minimum_required(VERSION 3.20)
project(glew_vendored LANGUAGES C)

add_library(glew_static STATIC src/glew.c)

target_include_directories(glew_static PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

# Static GLEW — must be PUBLIC so consumers compiling glew.h agree it's static.
target_compile_definitions(glew_static PUBLIC GLEW_STATIC)

# Links into your shared extension on Linux → needs PIC.
set_target_properties(glew_static PROPERTIES POSITION_INDEPENDENT_CODE ON)

# GLEW's own dependency: the platform OpenGL loader (opengl32 on Windows, libGL on Linux).
find_package(OpenGL REQUIRED)
target_link_libraries(glew_static PUBLIC OpenGL::GL)

add_library(GLEW::vendored ALIAS glew_static)