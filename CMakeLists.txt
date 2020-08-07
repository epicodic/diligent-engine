cmake_minimum_required (VERSION 3.6)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)
project(diligent-engine)

add_subdirectory(DiligentCore)
add_subdirectory(DiligentTools)
add_subdirectory(DiligentFX)

function(set_visibility_target_properties TARGET)
    set_target_properties(${TARGET} PROPERTIES
        CXX_VISIBILITY_PRESET default
        C_VISIBILITY_PRESET default
        VISIBILITY_INLINES_HIDDEN FALSE
    )
endfunction()

set_visibility_target_properties(Diligent-Common)
set_visibility_target_properties(Diligent-GraphicsEngineNextGenBase)
set_visibility_target_properties(Diligent-GraphicsAccessories)
set_visibility_target_properties(Diligent-GraphicsTools)
set_visibility_target_properties(Diligent-GraphicsEngine)
set_visibility_target_properties(Diligent-TextureLoader)
set_visibility_target_properties(Diligent-AssetLoader)
set_visibility_target_properties(DiligentFX)

add_library(diligent-engine SHARED empty_source.cpp)

target_link_libraries(diligent-engine
  PRIVATE
    -Wl,--whole-archive 
    Diligent-Common
    Diligent-GraphicsEngineNextGenBase 
    Diligent-GraphicsAccessories
    Diligent-GraphicsTools
    Diligent-GraphicsEngine
    Diligent-TextureLoader
    Diligent-AssetLoader
    DiligentFX
    # Diligent-Imgui
    -Wl,--no-whole-archive
)


add_library(diligent-engine-opengl SHARED empty_source.cpp)
target_link_libraries(diligent-engine-opengl
  PRIVATE
    -Wl,--whole-archive
    Diligent-GraphicsEngineOpenGL-static
    -Wl,--no-whole-archive
  PUBLIC
    diligent-engine
)

add_library(diligent-engine-vulkan SHARED empty_source.cpp)
target_link_libraries(diligent-engine-vulkan
  PRIVATE
    -Wl,--whole-archive
    Diligent-GraphicsEngineVk-static
    -Wl,--no-whole-archive
  PUBLIC
    diligent-engine
)

install(TARGETS	diligent-engine diligent-engine-opengl diligent-engine-vulkan 
        ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
        LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
        RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
)

set(CPACK_GENERATOR "DEB")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "epicodic@web.de")
set(CPACK_PACKAGING_INSTALL_PREFIX "/opt/diligent-engine")
set(CPACK_INSTALL_PREFIX "/opt/diligent-engine")
set(CPACK_DEBIAN_PACKAGE_NAME "diligent-engine")
set(CPACK_PACKAGE_VERSION "1.0")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "diligent-engine")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "diligent-engine")
set(CPACK_PACKAGE_NAME "diligent-engine")
set(CPACK_PACKAGE_VENDOR "diligent-engine")
#set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
set(CPACK_PACKAGE_CONTACT "Diligent Graphics <info@diligent-engineics.com>")

include(CPack)
