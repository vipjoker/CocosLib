
if(ANDROID)

set(COCOS_PLATFORM_SPECIFIC_SRC
  platform/android/CCApplication-android.cpp
  platform/android/CCCommon-android.cpp
  platform/android/CCGLViewImpl-android.cpp
  platform/android/CCFileUtils-android.cpp
  platform/android/CCEnhanceAPI-android.cpp
  platform/android/jni/JniHelper.cpp
)

set(COCOS2DX_ANDROID_STATIC
 android/CCDevice-android.cpp
 android/javaactivity-android.cpp
 android/jni/TouchesJni.cpp
 android/jni/Java_org_cocos2dx_lib_Cocos2dxAccelerometer.cpp
 android/jni/Java_org_cocos2dx_lib_Cocos2dxBitmap.cpp
 android/jni/Java_org_cocos2dx_lib_Cocos2dxHelper.cpp
 android/jni/Java_org_cocos2dx_lib_Cocos2dxRenderer.cpp
 android/jni/Java_org_cocos2dx_lib_Cocos2dxEngineDataManager.cpp
)
#
add_library(cocos2dxandroid_static STATIC ${COCOS2DX_ANDROID_STATIC})
set_property(TARGET cocos2dxandroid_static PROPERTY POSITION_INDEPENDENT_CODE TRUE)




set(internal_dir ${CMAKE_CURRENT_SOURCE_DIR}../../../lib/internal)





set_target_properties(cocos2dxandroid_static
        PROPERTIES
        ARCHIVE_OUTPUT_DIRECTORY
        "${internal_dir}/${ANDROID_ABI}")



endif()

#leave andatory external stuff here also

include_directories(

  ../external
  ../external/ConvertUTF
  ../external/edtaa3func
  ../external/poly2tri
  ../external/poly2tri/common
  ../external/poly2tri/sweep
)

set(COCOS_PLATFORM_SRC

  platform/CCSAXParser.cpp
  platform/CCThread.cpp
  platform/CCGLView.cpp
  platform/CCFileUtils.cpp
  platform/CCImage.cpp
  ../external/edtaa3func/edtaa3func.cpp
  ../external/ConvertUTF/ConvertUTFWrapper.cpp
  ../external/ConvertUTF/ConvertUTF.c
  ../external/md5/md5.c
  ../external/poly2tri/common/shapes.cc
  ../external/poly2tri/sweep/advancing_front.cc
  ../external/poly2tri/sweep/cdt.cc
  ../external/poly2tri/sweep/sweep_context.cc
  ../external/poly2tri/sweep/sweep.cc
  ${COCOS_PLATFORM_SPECIFIC_SRC}

)
