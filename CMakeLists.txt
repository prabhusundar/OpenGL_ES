cmake_minimum_required(VERSION 2.6)
project(OpenGLES_Samples)
include_directories(${OpenGLES_Samples_SOURCE_DIR}/GLUS/src/)

add_subdirectory(GLUS)
add_subdirectory(Example01_ES2)
add_subdirectory(Example02_ES2)
add_subdirectory(Example06_ES2)

