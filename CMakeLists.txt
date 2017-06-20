cmake_minimum_required(VERSION 2.8)

if(UNIX)
set(OpenCV_LIBS /usr/local/lib)
set(OpenCV_INCLUDE_DIRS /usr/local/include)
endif()

# if OpenCV2.4 is your choice, add paths like...
# set(OpenCV_DIR /Users/mhirano/opencv-2.4.13.2/build)           
# set(OpenCV_LIBS /Users/mhirano/opencv-2.4.13.2/build/lib)
# set(OpenCV_INCLUDE_DIRS /Users/mhirano/opencv-2.4.13.2/include)

project( SAMPLE CXX )
find_package( OpenCV REQUIRED )

# Check C++11 support and activate
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)

if(COMPILER_SUPPORTS_CXX11)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
	message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

set(CMAKE_CXX_FLAGS_DEBUG "-DDEBUG -g -pg")
message(STATUS "${CMAKE_CXX_FLAGS_DEBUG}")
set(CMAKE_CXX_FLAGS_RELEASE "-DRELEASE -O3 -DNDEBUG")
message(STATUS "${CMAKE_CXX_FLAGS_RELEASE}")

include_directories( ${OpenCV_INCLUDE_DIRS} ${PROJECT_SOURCE_DIR}/include )
message(STATUS "${PROJECT_SOURCE_DIR}")

set( src
		${PROJECT_SOURCE_DIR}/src/main.cpp
		)

add_executable( SAMPLE ${src} )
target_link_libraries( SAMPLE ${OpenCV_LIBS})
