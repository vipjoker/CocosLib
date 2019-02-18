
# this CMakeLists is to generate libcocos2d.a

cmake_minimum_required(VERSION 3.1)


include(2d/CMakeLists.txt)
include(platform/CMakeLists.txt)
include(math/CMakeLists.txt)
include(renderer/CMakeLists.txt)
include(base/CMakeLists.txt)
include(deprecated/CMakeLists.txt)
include(ui/CMakeLists.txt)
include(network/CMakeLists.txt)
include(audio/CMakeLists.txt)

include_directories(include
        ../external/xxtea
        ../external/clipper
        ../external/freetype2/include/android/freetype2
        ../external/unzip
        ../external/tinyxml2
        ../external/png/include/android
        ../external/tiff/include/android
        ../external/jpeg/include/android
        ../external/xxhash
        ../external/android-specific/tremolo
        ../external/android-specific/pvmp3dec/include
        ../external/android-specific/pvmp3dec/src
        ../external/curl/include/android
        ../external/websockets/include/android
        ../external/openssl/include/android

        )


if (BUILD_EDITOR_COCOSBUILDER)
    include(editor-support/cocosbuilder/CMakeLists.txt)
    set(COCOS_EDITOR_SUPPORT_SRC ${COCOS_EDITOR_SUPPORT_SRC} ${COCOS_CCB_SRC})
endif (BUILD_EDITOR_COCOSBUILDER)

if (BUILD_EDITOR_COCOSTUDIO)
    include(editor-support/cocostudio/CMakeLists.txt)
    set(COCOS_EDITOR_SUPPORT_SRC ${COCOS_EDITOR_SUPPORT_SRC} ${COCOS_CS_SRC})
endif (BUILD_EDITOR_COCOSTUDIO)

if (BUILD_EDITOR_SPINE)
    include(editor-support/spine/CMakeLists.txt)
    set(COCOS_EDITOR_SUPPORT_SRC ${COCOS_EDITOR_SUPPORT_SRC} ${COCOS_SPINE_SRC})
endif (BUILD_EDITOR_SPINE)

include(../extensions/CMakeLists.txt)

set(COCOS_SRC cocos2d.cpp
        ${COCOS_2D_SRC}
        ${COCOS_PLATFORM_SRC}
        ${COCOS_PHYSICS_SRC}
        ${COCOS_PHYSICS3D_SRC}
        ${COCOS_MATH_SRC}
        ${COCOS_NAVMESH_SRC}
        ${COCOS_RENDERER_SRC}
        ${COCOS_VR_SRC}
        ${COCOS_BASE_SRC}
        ${COCOS_AUDIO_SRC}
        ${COCOS_UI_SRC}
        ${COCOS_DEPRECATED_SRC}
        ${COCOS_NETWORK_SRC}
        ${COCOS_EDITOR_SUPPORT_SRC}
        ${COCOS_EXTENSIONS_SRC}
        )


add_library(cocos2dInternal ${COCOS_SRC})


set(distribution_DIR_lib ${CMAKE_CURRENT_SOURCE_DIR}../../lib/cocos2d_lib)


set_target_properties(cocos2dInternal PROPERTIES ARCHIVE_OUTPUT_DIRECTORY "${distribution_DIR_lib}/${ANDROID_ABI}")


#set_target_properties(cocos2dInternal
#    PROPERTIES
#    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
#    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
#    VERSION "${COCOS2D_X_VERSION}"
#)

set(PLATFORM_SPECIFIC_LIBS)
include(../cmake/Modules/CocosBuildHelpers.cmake)

add_subdirectory(../external/android-specific/tremolo "${CMAKE_BINARY_DIR}/lib/tremolo")
add_subdirectory(../external/android-specific/pvmp3dec "${CMAKE_BINARY_DIR}/lib/pvmp3dec")

set(PLATFORM_SPECIFIC_LIBS GLESv1_CM GLESv2 EGL log android OpenSLES tremolo pvmp3dec)
get_filename_component(__module_path "${CMAKE_TOOLCHAIN_FILE}" DIRECTORY)
include(../cmake/AndroidNdkModules.cmake)
android_ndk_import_module_cpufeatures()

foreach (pkg ZLIB MINIZIP JPEG PNG TIFF TinyXML2 FREETYPE WEBSOCKETS CURL OPENSSL FLATBUFFERS XXHASH)
    cocos_use_pkg(cocos2dInternal ${pkg})
endforeach ()


target_link_libraries(cocos2dInternal ${PLATFORM_SPECIFIC_LIBS} ${glfw_other_linker_flags})

if (USE_WEBP)
    add_definitions(-DCC_USE_WEBP=1)
    cocos_use_pkg(cocos2dInternal WEBP)
else ()
    add_definitions(-DCC_USE_WEBP=0)
endif ()


if (USE_BOX2D)
    cocos_use_pkg(cocos2dInternal Box2D)
endif ()

if (USE_BULLET)
    cocos_use_pkg(cocos2dInternal BULLET)
endif ()


ADD_LIBRARY(cocos2d cc_dummy.c)

if (ANDROID)
    target_link_libraries(cocos2d -Wl,-whole-archive cocos2dxandroid_static -Wl,-no-whole-archive)
    target_link_libraries(cocos2d cocos2dInternal cpufeatures)
endif ()

#set_target_properties(cocos2d
#    PROPERTIES
#    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
#    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
#    VERSION "${COCOS2D_X_VERSION}"
#)
#

set(distribution_DIR ${CMAKE_CURRENT_SOURCE_DIR}../../lib/cocos2d)


set_target_properties(cocos2d PROPERTIES ARCHIVE_OUTPUT_DIRECTORY "${distribution_DIR}/${ANDROID_ABI}")