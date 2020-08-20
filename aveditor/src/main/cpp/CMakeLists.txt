cmake_minimum_required(VERSION 3.4.1)


#设置输出的 so 目录
#set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/jniLibs/${ANDROID_ABI})

#-------------------------------------------------定义路径变量----------------------------------------------
#JNI 路径
set(JNI_PATH ${CMAKE_SOURCE_DIR})
#FFMPEG 头文件路径
set(FFMPEG_INCLUDE_PATH ${CMAKE_SOURCE_DIR}/../../../../thirdparties/ffmpeg)
# so 路径
set(SHARED_PATH ${CMAKE_SOURCE_DIR}/../jniLibs/${CMAKE_ANDROID_ARCH_ABI})
#xlog header 路径
set(XLOG_PATH ${CMAKE_SOURCE_DIR}/xlog)
#Base
set(BASE_PATH ${CMAKE_SOURCE_DIR}/base)
#变音变调
set(SOUNDTOUCH_PATH ${CMAKE_SOURCE_DIR}/soundtouch)


#-------------------------------------------------找文件----------------------------------------------
#找文件


#jni cpp
FILE(GLOB JNI_ALL_CPP ${JNI_PATH}/*.cpp)
FILE(GLOB JNI_ALL_H ${JNI_PATH}/*.h)
#解封装
FILE(GLOB DEMUX_ALL_CPP ${JNI_PATH}/demux/*.cpp)
FILE(GLOB DEMUX_ALL_H ${JNI_PATH}/demux/*.h)
#解码
FILE(GLOB DECODE_ALL_CPP ${JNI_PATH}/decode/*.cpp)
FILE(GLOB DECODE_ALL_H ${JNI_PATH}/decode/*.h)
#音频重采样
FILE(GLOB RESAMPLE_ALL_CPP ${JNI_PATH}/resample/*.cpp)
FILE(GLOB RESAMPLE_ALL_H ${JNI_PATH}/resample/*.h)
#音频渲染
FILE(GLOB AUDIO_ALL_CPP ${JNI_PATH}/audio/*.cpp)
FILE(GLOB AUDIO_ALL_H ${JNI_PATH}/audio/*.h)
#视频渲染
FILE(GLOB VIDEO_ALL_CPP ${JNI_PATH}/video/*.cpp)
FILE(GLOB VIDEO_ALL_H ${JNI_PATH}/video/*.h)
#构建模块
FILE(GLOB BUILDER_ALL_CPP ${JNI_PATH}/builder/*.cpp)
FILE(GLOB BUILDER_ALL_H ${JNI_PATH}/builder/*.h)
#base
FILE(GLOB BASE_ALL_CPP ${BASE_PATH}/*.cpp)
FILE(GLOB BASE_ALL_H ${BASE_PATH}/*.)
FILE(GLOB BASE_DATA_ALL_CPP ${BASE_PATH}/data/*.cpp)
FILE(GLOB BASE_DATA_ALL_H ${BASE_PATH}/data/*.h)
#播放模块
FILE(GLOB JNI_PATH_ALL_CPP ${JNI_PATH}/play/*.cpp)
FILE(GLOB JNI_PATH_ALL_H ${JNI_PATH}/play/*.h)
#回调模块
FILE(GLOB CALLBACK_PATH_ALL_CPP ${JNI_PATH}/callback/*.cpp)
FILE(GLOB CALLBACK_PATH_ALL_H ${JNI_PATH}/callback/*.h)
#utils 模块
FILE(GLOB UTILS_PATH_ALL_CPP ${JNI_PATH}/utils/*.cpp)
FILE(GLOB UTILS_PATH_ALL_H ${JNI_PATH}/utils/*.h)
#数据实体 模块
FILE(GLOB ENTITY_PATH_ALL_CPP ${JNI_PATH}/entity/*.cpp)
FILE(GLOB ENTITY_PATH_ALL_H ${JNI_PATH}/entity/*.h)
#封装音视频数据 mp4 flv 等
FILE(GLOB MUXER_PATH_ALL_CPP ${JNI_PATH}/muxer/*.cpp)
FILE(GLOB MUXER_PATH_ALL_H ${JNI_PATH}/muxer/*.h)
#soundtouch  path
FILE(GLOB SOUNDTOUCH_SOURCE_H_PATH ${CMAKE_SOURCE_DIR}/source/*.h)
FILE(GLOB SOUNDTOUCH_CPP_PATH ${CMAKE_SOURCE_DIR}/source/*.cpp)


#-------------------------------------------------加载头文件----------------------------------------------
#加载头文件
include_directories(${SOUNDTOUCH_PATH}/include)
include_directories(${SOUNDTOUCH_SOURCE_H_PATH})
include_directories(${FFMPEG_INCLUDE_PATH}/include)
include_directories(${XLOG_PATH}/include)
include_directories(${CMAKE_SOURCE_DIR})
include_directories(${DEMUX_ALL_H})
include_directories(${DECODE_ALL_H})
include_directories(${RESAMPLE_ALL_H})
include_directories(${AUDIO_ALL_H})
include_directories(${VIDEO_ALL_H})
include_directories(${BASE_ALL_H})
include_directories(${BASE_DATA_ALL_H})
include_directories(${BUILDER_ALL_H})
include_directories(${CALLBACK_PATH_ALL_H})
include_directories(${UTILS_PATH_ALL_H})
include_directories(${ENTITY_PATH_ALL_H})
include_directories(${MUXER_PATH_ALL_H})


#-------------------------------------------------加载库文件----------------------------------------------

add_library(avcodec SHARED IMPORTED)
set_target_properties(avcodec PROPERTIES IMPORTED_LOCATION ${SHARED_PATH}/libavcodec.so)

add_library(avformat SHARED IMPORTED)
set_target_properties(avformat PROPERTIES IMPORTED_LOCATION ${SHARED_PATH}/libavformat.so)

add_library(avutil SHARED IMPORTED)
set_target_properties(avutil PROPERTIES IMPORTED_LOCATION ${SHARED_PATH}/libavutil.so)

add_library(swscale SHARED IMPORTED)
set_target_properties(swscale PROPERTIES IMPORTED_LOCATION ${SHARED_PATH}/libswscale.so)

add_library(swresample SHARED IMPORTED)
set_target_properties(swresample PROPERTIES IMPORTED_LOCATION ${SHARED_PATH}/libswresample.so)

add_library(marsxlog SHARED IMPORTED)
set_target_properties(marsxlog PROPERTIES IMPORTED_LOCATION ${SHARED_PATH}/libmarsxlog.so)


add_library(
        avtools
        SHARED
        ${JNI_ALL_CPP}
        ${DEMUX_ALL_CPP}
        ${DECODE_ALL_CPP}
        ${RESAMPLE_ALL_CPP}
        ${AUDIO_ALL_CPP}
        ${VIDEO_ALL_CPP}
        ${BASE_ALL_CPP}
        ${BASE_DATA_ALL_CPP}
        ${BUILDER_ALL_CPP}
        ${JNI_PATH_ALL_CPP}
        ${CALLBACK_PATH_ALL_CPP}
        ${UTILS_PATH_ALL_CPP}
        ${ENTITY_PATH_ALL_CPP}
        ${MUXER_PATH_ALL_CPP}
        ${SOUNDTOUCH_CPP_PATH}
)


find_library(
        log-lib
        log
)


#AVTools
target_link_libraries(
        avtools
        GLESv2 EGL
        #[[EGL 是 nativeWindow 和 openGL ES 中间的桥梁]]
        OpenSLES
        android
        avcodec avformat avutil swscale swresample
        #log
        marsxlog
        ${log-lib}
)




