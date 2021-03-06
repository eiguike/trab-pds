# cd build
# cmake .. -DDEBUG=ON
# cmake .. -DDEBUG=OFF
cmake_minimum_required(VERSION 3.0)

project(digital)

find_path(FPRINT_INCLUDE NAMES libfprint/fprint.h)

find_library(LIBFPRINT libfprint.a)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -I${FPRINT_INCLUDE} ")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -I/usr/local/opt/openssl/include")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L/usr/local/opt/openssl/lib")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L/opt/local/lib")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L/usr/local/Cellar/nss/3.26/lib")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L/usr/local/Cellar/nspr/4.12_1/lib")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L/usr/local/Cellar/libusb/1.0.20/lib")

add_library(Digital STATIC Digital.cpp)
add_library(Device STATIC Device.cpp)

set(SOURCE_FILES main.cpp)
add_executable(digital ${SOURCE_FILES})

target_link_libraries(digital Digital)
target_link_libraries(Digital Device)

target_link_libraries(Digital /usr/local/lib/libfprint.a)
target_link_libraries(Digital glib-2.0)
target_link_libraries(Digital pixman-1)
target_link_libraries(Digital nss3)
target_link_libraries(Digital nssutil3)
target_link_libraries(Digital smime3)
target_link_libraries(Digital ssl3)
target_link_libraries(Digital plds4)
target_link_libraries(Digital plc4)
target_link_libraries(Digital nspr4)
target_link_libraries(Digital usb-1.0)
target_link_libraries(Digital crypto)
target_link_libraries(Digital curl)
target_link_libraries(Digital jsoncpp)

if(DEBUG)
  add_definitions(-DDEBUG)
  MESSAGE("DEBUG IS SET TO ON")
else()
  MESSAGE("DEBUG IS SET TO OFF")
  target_link_libraries(digital wiringPi)
endif()

install(TARGETS digital DESTINATION /bin)

