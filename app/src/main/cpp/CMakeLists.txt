cmake_minimum_required(VERSION 3.22.1)

set(NDK_PATH $ENV{ANDROID_NDK})
set(ANDROID_PLATFORM 34)
set(ANDROID_STL none)
#set(CMAKE_HOST_SYSTEM_NAME Windows)
set(CMAKE_BUILD_TYPE=Release)
set(ANDROID_ABI arm64-v8a)
set(triple x86_64-linux-android)

set(CMAKE_TOOLCHAIN_FILE: "/Users/tranluong/Library/Android/sdk/ndk/27.0.12077973/build/cmake")

set(PROJECT SecFfmpeg)
project(${PROJECT})

set(CMAKE_CXX_STANDARD 17)
file(REAL_PATH . PROJECT_PATH)

set(EXTERNAL_INCLUDE ${PROJECT_PATH}/mmfw_include)
set(EXTERNAL_LIBRARY ${PROJECT_PATH}/mmfw_library)

message("This is build for SecFfmpeg execute")
set(SecFfmpegExe secffmpeg)

set(LOCAL_SRC_FILES
        ${PROJECT_PATH}/fftools/cmdutils.c
        ${PROJECT_PATH}/fftools/ffmpeg.c
        ${PROJECT_PATH}/fftools/ffmpeg_dec.c
        ${PROJECT_PATH}/fftools/ffmpeg_demux.c
        ${PROJECT_PATH}/fftools/ffmpeg_enc.c
        ${PROJECT_PATH}/fftools/ffmpeg_filter.c
        ${PROJECT_PATH}/fftools/ffmpeg_hw.c
        ${PROJECT_PATH}/fftools/ffmpeg_mux.c
        ${PROJECT_PATH}/fftools/ffmpeg_mux_init.c
        ${PROJECT_PATH}/fftools/ffmpeg_opt.c
        ${PROJECT_PATH}/fftools/ffmpeg_sched.c
        ${PROJECT_PATH}/fftools/objpool.c
        ${PROJECT_PATH}/fftools/opt_common.c
        ${PROJECT_PATH}/fftools/sync_queue.c
        ${PROJECT_PATH}/fftools/thread_queue.c
)

add_library(${SecFfmpegExe}
        SHARED
        ${LOCAL_SRC_FILES}
)

target_compile_options(${SecFfmpegExe}
PRIVATE -g -O0
)

target_include_directories(
        ${SecFfmpegExe}
        PRIVATE
        ${EXTERNAL_INCLUDE}/ffmpeg/arm64-v8a
)

target_link_libraries(${SecFfmpegExe} PRIVATE
        ${EXTERNAL_LIBRARY}/sharedLibraries/arm64-v8a/libavcodec.so
        ${EXTERNAL_LIBRARY}/sharedLibraries/arm64-v8a/libavdevice.so
        ${EXTERNAL_LIBRARY}/sharedLibraries/arm64-v8a/libavfilter.so
        ${EXTERNAL_LIBRARY}/sharedLibraries/arm64-v8a/libavformat.so
        ${EXTERNAL_LIBRARY}/sharedLibraries/arm64-v8a/libavutil.so
        ${EXTERNAL_LIBRARY}/sharedLibraries/arm64-v8a/libpostproc.so
        ${EXTERNAL_LIBRARY}/sharedLibraries/arm64-v8a/libswresample.so
        ${EXTERNAL_LIBRARY}/sharedLibraries/arm64-v8a/libswscale.so
)

set(RUNTIME_OUTPUT_PATH "${PROJECT_PATH}/output/")
file(MAKE_DIRECTORY ${RUNTIME_OUTPUT_PATH})
set_target_properties(
        ${SecFfmpegExe} PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY ${RUNTIME_OUTPUT_PATH}
)

set(LIBRARY_OUTPUT_PATH "${PROJECT_PATH}/output/${ANDROID_ABI}")
file(MAKE_DIRECTORY ${LIBRARY_OUTPUT_PATH})
set_target_properties(
        ${SecFfmpegExe} PROPERTIES
        LIBRARY_OUTPUT_DIRECTORY ${LIBRARY_OUTPUT_PATH}
)
