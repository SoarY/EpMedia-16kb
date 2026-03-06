cmake_minimum_required(VERSION 3.22.1)

# 设置工程名称和语言
project(ffmpeg_exec LANGUAGES C)

# 当前jni目录（里面有 config.h）
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# 项目自带include
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

# FFmpeg源码
include_directories("F:/Code/other/ffmpeg-master/ffmpeg-4.2.2")
include_directories("F:/Code/other/ffmpeg-master/ffmpeg-4.2.2/fftools")


# ==== 2. 添加预编译的共享库 ====
# 这些对应原来的 LOCAL_MODULE + PREBUILT_SHARED_LIBRARY
add_library(libavutil SHARED IMPORTED)
set_target_properties(libavutil PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/prebuilt/libavutil.so")

add_library(libswresample SHARED IMPORTED)
set_target_properties(libswresample PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/prebuilt/libswresample.so")

add_library(libswscale SHARED IMPORTED)
set_target_properties(libswscale PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/prebuilt/libswscale.so")

add_library(libavcodec SHARED IMPORTED)
set_target_properties(libavcodec PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/prebuilt/libavcodec.so")

add_library(libavformat SHARED IMPORTED)
set_target_properties(libavformat PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/prebuilt/libavformat.so")

add_library(libavfilter SHARED IMPORTED)
set_target_properties(libavfilter PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/prebuilt/libavfilter.so")

add_library(libavdevice SHARED IMPORTED)
set_target_properties(libavdevice PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/prebuilt/libavdevice.so")

add_library(libpostproc SHARED IMPORTED)
set_target_properties(libpostproc PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/prebuilt/libpostproc.so")

# ==== 3. 编译自己的 C 文件为共享库 ====
add_library(ffmpeg-exec SHARED
        Jni_FFmpegCmd.c
        cmdutils.c
        ffmpeg_filter.c
        ffmpeg_opt.c
        F:/Code/other/ffmpeg-master/ffmpeg-4.2.2/fftools/ffmpeg_hw.c
        ffmpeg.c
        ffmpeg_thread.c
)

# ==== 4. 链接系统库 ====
target_link_libraries(ffmpeg-exec
        log
        jnigraphics
        z
        android
        m
        libavcodec
        libavfilter
        libavformat
        libavutil
        libswresample
        libswscale
        libavdevice
)

# ==== 5. 增加 16KB page size 支持 ====
target_link_options(ffmpeg-exec PRIVATE "-Wl,-z,max-page-size=16384")
