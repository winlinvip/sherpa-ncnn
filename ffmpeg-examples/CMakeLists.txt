
include_directories(${CMAKE_SOURCE_DIR})
add_executable(sherpa-ncnn-ffmpeg sherpa-ncnn-ffmpeg.cc)

# Link libraries from sherpa-ncnn.
target_link_libraries(sherpa-ncnn-ffmpeg sherpa-ncnn-c-api)

if (NOT SHERPA_NCNN_ENABLE_FFMPEG_PRIVATE)
    find_package(PkgConfig REQUIRED)
    pkg_check_modules(AVCODEC REQUIRED libavcodec)
    include_directories(${AVCODEC_INCLUDE_DIRS})
    target_link_directories(sherpa-ncnn-ffmpeg PRIVATE ${AVCODEC_LIBRARY_DIRS})
    # All libraries of FFmpeg shares the same include and library directory.
    # Note that ${AVCODEC_LIBRARIES} equals to avcodec, but we add it for consistence.
    target_link_libraries(sherpa-ncnn-ffmpeg avformat avfilter avcodec avutil swresample swscale avdevice)
elseif (APPLE)
    set(FFMPEG_HOME $ENV{HOME}/.release)
    set(LIBX11_HOME /usr/local/Cellar/libx11/1.8.4)
    set(LIBXCB_HOME /usr/local/Cellar/libxcb/1.15_1)
    include_directories(${FFMPEG_HOME}/include)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -L${FFMPEG_HOME}/lib -L${LIBX11_HOME}/lib -lavformat -lm -lbz2 -lz -Wl,-framework,CoreFoundation -Wl,-framework,Security -lavcodec -liconv -lm -llzma -framework AudioToolbox -pthread -lz -framework VideoToolbox -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices -lswresample -lm -lavutil -pthread -lm -framework VideoToolbox -lX11 -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -L${FFMPEG_HOME}/lib -L${LIBX11_HOME}/lib -lavfilter -framework OpenGL -pthread -lm -framework Metal -framework VideoToolbox -framework CoreImage -framework AppKit -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices -lswscale -lm -lavformat -lm -lbz2 -lz -Wl,-framework,CoreFoundation -Wl,-framework,Security -lavcodec -liconv -lm -llzma -framework AudioToolbox -pthread -lz -framework VideoToolbox -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices -lswresample -lm -lavutil -pthread -lm -framework VideoToolbox -lX11 -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -L${FFMPEG_HOME}/lib -L${LIBX11_HOME}/lib -lavcodec -liconv -lm -llzma -framework AudioToolbox -pthread -lz -framework VideoToolbox -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices -lswresample -lm -lavutil -pthread -lm -framework VideoToolbox -lX11 -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -L${FFMPEG_HOME}/lib -L${LIBX11_HOME}/lib -lavutil -pthread -lm -framework VideoToolbox -lX11 -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -L${FFMPEG_HOME}/lib -L${LIBX11_HOME}/lib -lswresample -lm -lavutil -pthread -lm -framework VideoToolbox -lX11 -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -L${FFMPEG_HOME}/lib -L${LIBX11_HOME}/lib -lswscale -lm -lavutil -pthread -lm -framework VideoToolbox -lX11 -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -L${FFMPEG_HOME}/lib -L${LIBXCB_HOME}/lib -L/usr/local/lib -L${FFMPEG_HOME}/lib -L${LIBX11_HOME}/lib -lavdevice -framework Foundation -framework AudioToolbox -framework CoreAudio -lm -framework AVFoundation -framework CoreVideo -framework CoreMedia -framework CoreGraphics -lxcb -lxcb-shm -lxcb-shape -lxcb-xfixes -framework AudioToolbox -pthread -lSDL2 -lavfilter -framework OpenGL -pthread -lm -framework Metal -framework VideoToolbox -framework CoreImage -framework AppKit -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices -lswscale -lm -lavformat -lm -lbz2 -lz -Wl,-framework,CoreFoundation -Wl,-framework,Security -lavcodec -liconv -lm -llzma -framework AudioToolbox -pthread -lz -framework VideoToolbox -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices -lswresample -lm -lavutil -pthread -lm -framework VideoToolbox -lX11 -framework CoreFoundation -framework CoreMedia -framework CoreVideo -framework CoreServices")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS}")
endif ()

