cmake_minimum_required(VERSION 3.2.0)
project(kraken_video_player)

set (CMAKE_MODULE_PATH /usr/local/lib/node_modules/kraken-npbt/cmake)
find_package(kraken)

add_library(kraken_video_player SHARED
  media_element.cc
media_element.h
video_element.cc
video_element.h
)

if ($ENV{KRAKEN_JS_ENGINE} MATCHES "jsc")
  set_target_properties(kraken_video_player PROPERTIES OUTPUT_NAME kraken_video_player_jsc)
endif()

list(APPEND BRIDGE_INCLUDE ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(kraken_video_player ${BRIDGE_LINK_LIBS} kraken)
target_include_directories(kraken_video_player PUBLIC ${BRIDGE_INCLUDE})

if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set_target_properties(kraken_video_player
        PROPERTIES
        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../macos"
        )
elseif(${CMAKE_SYSTEM_NAME} MATCHES "Android")
  set_target_properties(kraken_video_player
        PROPERTIES
        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../android/jniLibs/${ANDROID_ABI}/"
        )
endif()
