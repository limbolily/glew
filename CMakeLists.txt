cmake_minimum_required(VERSION 2.8)

include("yong_injector/injector.cmake")

yong_project("glew" C)

set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)
set(INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)

include_directories(${INCLUDE_DIR})

set(GLEW_SOURCES
  ${SRC_DIR}/glew.c
  )

if (WIN32)
  add_definitions(-DWIN32_MEAN_AND_LEAN -DVC_EXTRALEAN -DGLEW_BUILD -D_CRT_SECURE_NO_WARNINGS)
endif()

yong_option(GLEW_USE_GLU "Check if use glu" OFF)
if (NOT GLEW_USE_GLU)
  add_definitions(-DGLEW_NO_GLU)
endif()

find_package(OpenGL REQUIRED)

if (BUILD_SHARED_LIBS)
  yong_add_library(glew
    LIB_TYPE SHARED
    SOURCES ${GLEW_SOURCES})
  target_link_libraries(glew ${OPENGL_gl_LIBRARY})
  if (GLEW_USE_GLU)
    target_link_libraries(glew ${OPENGL_glu_LIBRARY})
  endif ()
else ()
  add_definitions(-DGLEW_STATIC)
  yong_add_library(glew
    LIB_TYPE STATIC
    SOURCES ${GLEW_SOURCES})
endif ()

yong_install_header_directories(glew
  DIRECTORIES include/GL)
yong_add_library_end(glew)

yong_project_end()
