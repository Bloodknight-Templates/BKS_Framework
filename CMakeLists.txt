cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
project(BKSFramework)

##### Project files
# Add source files
file(GLOB_RECURSE SOURCE_FILES 
	${CMAKE_SOURCE_DIR}/src/*.c
	${CMAKE_SOURCE_DIR}/src/*.cpp)

# Add header files
file(GLOB_RECURSE HEADER_FILES 
	${CMAKE_SOURCE_DIR}/src/*.h
	${CMAKE_SOURCE_DIR}/src/*.hpp)

# Add Dear ImGui
set(DEAR_IMGUI_PREFIX "${CMAKE_SOURCE_DIR}/include/imgui" CACHE STRING "Path to Dear ImGui")
# add Dear ImGui sources manually
list(APPEND SOURCE_FILES
	${DEAR_IMGUI_PREFIX}/imgui.cpp
  #${DEAR_IMGUI_PREFIX}/imgui_stdlib.cpp
	${DEAR_IMGUI_PREFIX}/imgui_draw.cpp
	${DEAR_IMGUI_PREFIX}/imgui_tables.cpp
	${DEAR_IMGUI_PREFIX}/imgui_widgets.cpp
	${DEAR_IMGUI_PREFIX}/imgui_impl_glfw.cpp
	${DEAR_IMGUI_PREFIX}/imgui_impl_opengl3.cpp
	${DEAR_IMGUI_PREFIX}/imgui_demo.cpp
)

# Add .lib files
link_directories(${CMAKE_SOURCE_DIR}/lib) 

# Configure assets header file
configure_file(cmake/helpers/RootDir.h.in cmake/helpers/RootDir.h)
include_directories(${CMAKE_BINARY_DIR}/src)

# Define the executable
add_executable(${PROJECT_NAME} ${HEADER_FILES} ${SOURCE_FILES})
#####


#####  External libraries
# We need a CMAKE_DIR with some code to find external dependencies
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

# OpenGL
find_package(OpenGL REQUIRED)

# GLM
find_package(GLM REQUIRED)
message(STATUS "GLM included at ${GLM_INCLUDE_DIR}")

# GLFW
find_package(GLFW3 REQUIRED)
message(STATUS "Found GLFW3 in ${GLFW3_INCLUDE_DIR}")

# STB_IMAGE
add_library(STB_IMAGE "thirdparty/stb_image.cpp")

# GLAD
add_library(GLAD "thirdparty/glad.c")

# Put all libraries into a variable
set(LIBS glfw3 opengl32 STB_IMAGE GLAD)

# Define the include DIRs
include_directories(
	"${CMAKE_SOURCE_DIR}/src"
	"${CMAKE_SOURCE_DIR}/include"
)

# Define the link libraries
target_link_libraries(${PROJECT_NAME} ${LIBS})
#####


##### Visual Studio filters
# Create virtual folders to make it look nicer in VS
if(MSVC_IDE)
	# Macro to preserve source files hierarchy in the IDE
	macro(GroupSources curdir)
		file(GLOB children RELATIVE ${PROJECT_SOURCE_DIR}/${curdir} ${PROJECT_SOURCE_DIR}/${curdir}/*)

		foreach(child ${children})
			if(IS_DIRECTORY ${PROJECT_SOURCE_DIR}/${curdir}/${child})
				GroupSources(${curdir}/${child})
			else()
				string(REPLACE "/" "\\" groupname ${curdir})
				string(REPLACE "src" "Sources" groupname ${groupname})
				source_group(${groupname} FILES ${PROJECT_SOURCE_DIR}/${curdir}/${child})
			endif()
		endforeach()
	endmacro()

	# Run macro
	GroupSources(src)
endif()
#####







