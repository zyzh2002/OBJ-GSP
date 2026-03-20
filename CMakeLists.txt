cmake_minimum_required(VERSION 3.22)

project(OBJ-GSP)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(GFLAGS_USE_TARGET_NAMESPACE ON)

find_package(OpenCV REQUIRED)
find_package(OpenMP REQUIRED)
find_package(onnxruntime CONFIG REQUIRED)
find_package(Eigen3 CONFIG REQUIRED)
find_package(vlfeat CONFIG REQUIRED)
find_package(gflags CONFIG REQUIRED)

add_subdirectory(Debugger)
add_subdirectory(Feature)
add_subdirectory(Mesh)
add_subdirectory(Stitching)
add_subdirectory(Util)
add_subdirectory(sam)

add_library(obj_gsp_core INTERFACE)
target_include_directories(obj_gsp_core
	INTERFACE
		${CMAKE_SOURCE_DIR}
		${OpenCV_INCLUDE_DIRS}
)
target_link_libraries(obj_gsp_core
	INTERFACE
		samLib
		debuggerLib
		featureLib
		meshLib
		stitchingLib
		utilLib
		${OpenCV_LIBS}
		OpenMP::OpenMP_CXX
		onnxruntime::onnxruntime
		Eigen3::Eigen
		unofficial::vlfeat::vl
		gflags::gflags
)

add_executable(main main.cpp)
target_link_libraries(main PRIVATE obj_gsp_core)

option(OBJ_GSP_BUILD_SAM_TEST "Build SAM standalone test executable" OFF)

if(OBJ_GSP_BUILD_SAM_TEST)
	add_executable(sam_test sam/sam_test.cpp)
	target_link_libraries(sam_test
		PRIVATE
			samLib
			${OpenCV_LIBS}
			onnxruntime::onnxruntime
			gflags::gflags
	)
endif()
