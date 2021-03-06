cmake_minimum_required(VERSION 3.0)

project( SAMPLE CXX )

#### Environment
if (UNIX AND NOT APPLE) #Ubuntu...
	message(STATUS ">>> Linux")

	set(OpenCV_DIR /home/mhirano/opencv-3.2.0/build)

	include_directories(/usr/include)
	link_directories(/usr/lib/x86_64-linux-gnu)

else(APPLE)
	message(STATUS ">>> Mac")

	set(OpenCV_DIR /Users/mhirano/opencv-4.3.0/build)

	# Add include directory	(environment specific)
	include_directories(/usr/local/include)

endif()

# OpenCV
find_package( OpenCV REQUIRED )
include_directories( ${OpenCV_INCLUDE_DIRS} ${PROJECT_SOURCE_DIR}/include )

######## ######## ######## ######## ######## ######## ######## ########
# Compiler settings
######## ######## ######## ######## ######## ######## ######## ########
# Check C++14 support and activate
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++14" COMPILER_SUPPORTS_CXX14)
if(COMPILER_SUPPORTS_CXX14)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")
	set(CMAKE_CXX_STANDARD 14) # C++14...
	set(CMAKE_CXX_STANDARD_REQUIRED ON) #...is required...
	set(CMAKE_CXX_EXTENSIONS OFF) #...without compiler extensions like gnu++11
	message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has C++14 support.")
else()
	message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++14 support. Please use a different C++ compiler.")
endif()

set(CMAKE_CXX_FLAGS_DEBUG "-DDEBUG -g")
message(STATUS "${CMAKE_CXX_FLAGS_DEBUG}")
set(CMAKE_CXX_FLAGS_RELEASE "-DRELEASE -O3 -DNDEBUG -march=native")
message(STATUS "${CMAKE_CXX_FLAGS_RELEASE}")
######## ######## ######## ######## ######## ######## ######## ########

set( src
		${PROJECT_SOURCE_DIR}/src/main.cpp
		)

add_executable( SAMPLE ${src} )
target_link_libraries( SAMPLE ${OpenCV_LIBS})
