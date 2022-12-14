cmake_minimum_required(VERSION 3.0…3.16)
project(vivoe-media)
include(FindPkgConfig)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -Wall -Werror")

if (${CMAKE_SYSTEM_NAME} MATCHES "MSYS")
  message("Detected MSYS2...")
  set(MSYS_LIBS ws2_32 mingwex)
endif()

add_library(vivoe-media SHARED rtp_stream.cc )
pkg_check_modules(SWSCALE REQUIRED libswscale)
target_link_libraries(vivoe-media png pthread ${SWSCALE_LIBRARIES} ${MSYS_LIBS})
target_include_directories(vivoe-media PUBLIC ${SWSCALE_INCLUDE_DIRS})
set_target_properties(vivoe-media PROPERTIES SOVERSION 1)

project(rtp-example)
add_executable(rtp-example example.cc pngget.cc)
target_link_libraries(rtp-example vivoe-media)
file(COPY lenna-lg.png DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

# Install 
install(TARGETS vivoe-media DESTINATION /usr/local/lib)
