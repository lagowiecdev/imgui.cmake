#
# ImGui Library
#
cmake_minimum_required(VERSION 3.15)

project(imgui)

if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    message(STATUS "[ImGui] Default install path: [${CMAKE_BINARY_DIR}/dist]")
    set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/dist" CACHE PATH "" FORCE)
endif()

set(IMGUI_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/imgui")
set(BACKENDS_DIR "${IMGUI_SOURCE_DIR}/backends")

set(PUBLIC_H_FILES
        "${IMGUI_SOURCE_DIR}/imgui.h"
        "${IMGUI_SOURCE_DIR}/imconfig.h"
        "${IMGUI_SOURCE_DIR}/imgui_internal.h"
        "${IMGUI_SOURCE_DIR}/imstb_rectpack.h"
        "${IMGUI_SOURCE_DIR}/imstb_textedit.h"
        "${IMGUI_SOURCE_DIR}/imstb_truetype.h"
)

set(PUBLIC_CXX_FILES
        ${IMGUI_SOURCE_DIR}/imgui.cpp
        ${IMGUI_SOURCE_DIR}/imgui_draw.cpp
        ${IMGUI_SOURCE_DIR}/imgui_widgets.cpp
        ${IMGUI_SOURCE_DIR}/imgui_demo.cpp
        ${IMGUI_SOURCE_DIR}/imgui_tables.cpp
)

file(GLOB FONTS_FILES ${IMGUI_SOURCE_DIR}/misc/fonts/*.ttf)

set(IMGUI_INTERFACE_LIBS "")
if(IMGUI_WITH_BACKEND)
    if("${IMGUI_BACKEND_PLATFORM}" STREQUAL "WIN32")
        message(STATUS "[ImGui] Including backend platform: [WIN32]")
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_win32.h)
        list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_win32.cpp)
        list(APPEND IMGUI_INTERFACE_LIBS dwmapi)
    endif()
    if("${IMGUI_BACKEND_PLATFORM}" STREQUAL "APPLE")
        message(STATUS "[ImGui] Including backend platform: [APPLE]")
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_osx.h)
        list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_osx.mm)
    endif()
    if (IMGUI_BACKEND_GLFW)
        message(STATUS "[ImGui] Including backend api: [GLFW]")
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_glfw.h)
        list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_glfw.cpp)
        list(APPEND IMGUI_INTERFACE_LIBS glfw)
    endif ()
    if (IMGUI_BACKEND_SDL2)
        message(STATUS "[ImGui] Including backend api: [SDL2]")
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_sdl2.h)
        list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_sdl2.cpp)
        list(APPEND IMGUI_INTERFACE_LIBS SDL2)
    endif ()
    if (IMGUI_BACKEND_SDL3)
        message(STATUS "[ImGui] Including backend api: [SDL3]")
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_sdl3.h)
        list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_sdl3.cpp)
        list(APPEND IMGUI_INTERFACE_LIBS SDL3)
    endif ()
    if(IMGUI_BACKEND_DX9)
        message(STATUS "[ImGui] Including backend api: [Direct3D9]")
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_dx9.h)
        list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_dx9.cpp)
        list(APPEND IMGUI_INTERFACE_LIBS d3d9)
    endif()
    if(IMGUI_BACKEND_DX10)
        message(STATUS "[ImGui] Including backend api: [Direct3D10]")
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_dx10.h)
        list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_dx10.cpp)
        list(APPEND IMGUI_INTERFACE_LIBS d3d10)
    endif()
    if(IMGUI_BACKEND_DX11)
        message(STATUS "[ImGui] Including backend api: [Direct3D11]")
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_dx11.h)
        list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_dx11.cpp)
        list(APPEND IMGUI_INTERFACE_LIBS d3d11)
    endif()
    if(IMGUI_BACKEND_DX12)
        message(STATUS "[ImGui] Including backend api: [Direct3D12]")
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_dx12.h)
        list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_dx12.cpp)
        list(APPEND IMGUI_INTERFACE_LIBS d3d12)
        list(APPEND IMGUI_INTERFACE_LIBS dxgi)
        # list(APPEND IMGUI_INTERFACE_LIBS d3dcompiler)
    endif()
    if (IMGUI_BACKEND_OPENGL)
        message(STATUS "[ImGui] Including backend api: [OpenGL]")
        #list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_opengl2.h)
        #list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_opengl2.cpp)
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_opengl3.h)
        list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_opengl3.cpp)
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_opengl3_loader.h)
        #list(APPEND IMGUI_INTERFACE_LIBS opengl)
    endif ()
    if (IMGUI_BACKEND_VULKAN)
        message(STATUS "[ImGui] Including backend api: [Vulkan]")
        list(APPEND PUBLIC_H_FILES ${BACKENDS_DIR}/imgui_impl_vulkan.h)
        list(APPEND PUBLIC_CXX_FILES ${BACKENDS_DIR}/imgui_impl_vulkan.cpp)
    endif ()
endif()

set(PROJECT_FILES
  ${PUBLIC_H_FILES}
  ${PUBLIC_CXX_FILES}
  ${FONTS_FILES}
)

##################################################################################################################
# TARGET
##################################################################################################################

add_library("${PROJECT_NAME}" ${PROJECT_FILES})

target_include_directories(
  "${PROJECT_NAME}"
  PUBLIC
  "${IMGUI_SOURCE_DIR}/"
  "${BACKENDS_DIR}/"
)

target_link_libraries("${PROJECT_NAME}" PUBLIC "${IMGUI_INTERFACE_LIBS}")

set_target_properties(
  "${PROJECT_NAME}"
  PROPERTIES
  PREFIX ""
)