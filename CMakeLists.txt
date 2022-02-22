#[[cmake_minimum_required(VERSION 3.21)
project(linux_project)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "-D__STDC_CONSTANT_MACROS")

find_package(PkgConfig REQUIRED)
find_package(FFmpeg COMPONENTS AVCODEC AVFORMAT AVUTIL AVDEVICE REQUIRED) #add here the list of ffmpeg components required

pkg_check_modules(AVCODEC    REQUIRED IMPORTED_TARGET libavcodec)
pkg_check_modules(AVFORMAT   REQUIRED IMPORTED_TARGET libavformat)
pkg_check_modules(AVFILTER   REQUIRED IMPORTED_TARGET libavfilter)
pkg_check_modules(AVDEVICE   REQUIRED IMPORTED_TARGET libavdevice)
pkg_check_modules(AVUTIL     REQUIRED IMPORTED_TARGET libavutil)
pkg_check_modules(SWRESAMPLE REQUIRED IMPORTED_TARGET libswresample)
pkg_check_modules(SWSCALE    REQUIRED IMPORTED_TARGET libswscale)

add_library(FFmpeg INTERFACE IMPORTED GLOBAL)

add_executable(linux_project main.cpp
        VideoAudioRecorder.h VideoAudioRecorder.cpp
        AudioRecorder.h AudioRecorder.cpp
        VideoRecorder.h VideoRecorder.cpp
        ffmpeg.h
        )

target_link_libraries(FFmpeg INTERFACE PkgConfig::AVCODEC)
target_link_libraries(FFmpeg INTERFACE PkgConfig::AVFORMAT)
target_link_libraries(FFmpeg INTERFACE PkgConfig::AVFILTER)
target_link_libraries(FFmpeg INTERFACE PkgConfig::AVDEVICE)
target_link_libraries(FFmpeg INTERFACE PkgConfig::AVUTIL)
target_link_libraries(FFmpeg INTERFACE PkgConfig::SWRESAMPLE)
target_link_libraries(FFmpeg INTERFACE PkgConfig::SWSCALE)
]]



#[[cmake_minimum_required(VERSION 3.21)
project(PDS_screen_capture_linux)

set(CMAKE_CXX_STANDARD 17)
#set(CMAKE_CXX_FLAGS "-D__STDC_CONSTANT_MACROS")


message(STATUS "SONO QUIIIII")

find_path(AVCODEC_INCLUDE_DIR include/libavcodec/avcodec.h)
find_library(AVCODEC_LIBRARY lib/avcodec)

find_path(AVFORMAT_INCLUDE_DIR include/libavformat/avformat.h)
find_library(AVFORMAT_LIBRARY lib/avformat)

find_path(AVUTIL_INCLUDE_DIR include/libavutil/avutil.h)
find_library(AVUTIL_LIBRARY lib/avutil)

find_path(AVDEVICE_INCLUDE_DIR include/libavdevice/avdevice.h)
find_library(AVDEVICE_LIBRARY lib/avdevice)

add_executable(PDS_screen_capture_linux main.cpp
        VideoAudioRecorder.h VideoAudioRecorder.cpp
        AudioRecorder.h AudioRecorder.cpp
        VideoRecorder.h VideoRecorder.cpp
        )

message(STATUS "AVCODEC_INCLUDE_DIR=${AVCODEC_INCLUDE_DIR}")
message(STATUS "AVCODEC_LIBRARY=${AVCODEC_LIBRARY}")
message(STATUS "AVDEVICE_INCLUDE_DIR=${AVDEVICE_INCLUDE_DIR}")
message(STATUS "AVDEVICE_LIBRARY=${AVDEVICE_LIBRARY}")

target_include_directories(PDS_screen_capture_linux PRIVATE ${AVCODEC_INCLUDE_DIR} ${AVFORMAT_INCLUDE_DIR} ${AVUTIL_INCLUDE_DIR} ${AVDEVICE_INCLUDE_DIR})
target_link_libraries(PDS_screen_capture_linux PRIVATE ${AVCODEC_LIBRARY} ${AVFORMAT_LIBRARY} ${AVUTIL_LIBRARY} ${AVDEVICE_LIBRARY})
]]
#[[cmake_minimum_required(VERSION 3.21)
project(linux_project)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "-D__STDC_CONSTANT_MACROS")

INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR})

message(STATUS "PROJECT_SOURCE_DIR=${PROJECT_SOURCE_DIR}")
SET (FFMPEG_DIR /include)
SET (FFMPEG_DIRS
        ${FFMPEG_DIR}
        ${FFMPEG_DIR}/libavcodec
        ${FFMPEG_DIR}/libavdevice
        ${FFMPEG_DIR}/libavfilter
        ${FFMPEG_DIR}/libavformat
        ${FFMPEG_DIR}/libavutil
        ${FFMPEG_DIR}/libavswresample
        ${FFMPEG_DIR}/libswscale
        )

message(STATUS "FFMPEG_DIR=${FFMPEG_DIR}")

INCLUDE_DIRECTORIES(${FFMPEG_DIRS})
INCLUDE_DIRECTORIES(${CMAKE_BINARY_DIR})

LINK_DIRECTORIES(${FFMPEG_DIRS})


SET (myprj_src main.cpp)

SET (FFMPEG_LIBRARIES
        lib/libavcodec
        lib/libavformat
        lib/libavutil
        lib/libavswresample
        lib/libswscale
        )

ADD_EXECUTABLE(EncDec ${myprj_src})
TARGET_LINK_LIBRARIES(EncDec ${FFMPEG_LIBRARIES})
]]

cmake_minimum_required(VERSION 3.20)
project(linux_project)

set(CMAKE_CXX_STANDARD 17)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")

find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBAV REQUIRED IMPORTED_TARGET
        libavcodec
        libavdevice
        libavfilter
        libavformat
        libavutil
        libswresample
        libswscale
        )

add_executable(linux_project main.cpp
        VideoAudioRecorder.h VideoAudioRecorder.cpp
        AudioRecorder.h AudioRecorder.cpp
        VideoRecorder.h VideoRecorder.cpp
        )

target_link_libraries(${PROJECT_NAME}
        PkgConfig::LIBAV
        )