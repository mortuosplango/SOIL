cmake_minimum_required (VERSION 2.6)
project (SOIL)

set (SOIL_BUILD_TESTS false CACHE BOOL "Build tests")

find_package (OpenGL REQUIRED)
include_directories (${OPENGL_INCLUDE_DIR})

file (GLOB SOIL_SOURCES src/*.c)
file (GLOB SOIL_HEADERS src/*.h)

add_library (SOIL SHARED ${SOIL_SOURCES} ${SOIL_HEADERS})

if(APPLE)
  target_link_libraries (SOIL ${OPENGL_LIBRARIES} "-framework CoreFoundation")
else(APPLE)
  target_link_libraries (SOIL ${OPENGL_LIBRARIES})
endif(APPLE)

install (TARGETS SOIL
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib)
install (FILES ${SOIL_HEADERS} DESTINATION include/SOIL)

if (SOIL_BUILD_TESTS)
	add_executable (test_SOIL src/test_SOIL.cpp)
	target_link_libraries (test_SOIL SOIL)
	install (TARGETS SOIL test_SOIL RUNTIME DESTINATION bin)
endif ()
