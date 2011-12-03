cmake_minimum_required(VERSION 2.8)

project(jpeg)

if(UNIX AND CMAKE_SYSTEM_NAME MATCHES Linux)
    set(LINUX ON)
endif()
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(M64 ON)
else()
    set(M32 ON)
endif()

file(GLOB jpeg_HEADERS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} *.h)
file(GLOB jpeg_SOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} *.c)
list(REMOVE_ITEM jpeg_SOURCES
    ansi2knr.c
    cjpeg.c
    ckconfig.c
    djpeg.c
    jpegtran.c
    rdjpgcom.c
    wrjpgcom.c
    jmemdos.c
    jmemmac.c
)

if(LINUX AND M64)
    list(APPEND jpeg_COMPILE_FLAGS "-fPIC")
endif()

add_library(jpeg STATIC ${jpeg_SOURCES} ${jpeg_HEADERS})

if(jpeg_COMPILE_FLAGS)
    set_property(TARGET jpeg PROPERTY COMPILE_FLAGS ${jpeg_COMPILE_FLAGS})
endif()

install(TARGETS jpeg EXPORT jpeg-exports DESTINATION lib)
install(FILES ${jpeg_HEADERS} DESTINATION include)
install(FILES jpeg-config.cmake DESTINATION share/jpeg)
install(EXPORT jpeg-exports DESTINATION share/jpeg)
