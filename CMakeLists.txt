cmake_minimum_required(VERSION 3.22)

project(OBJ-GSP)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(GFLAGS_USE_TARGET_NAMESPACE ON)

find_package(OpenCV REQUIRED)
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

add_library(obj_gsp_sam STATIC ${OBJ_GSP_SAM_SOURCES})
target_include_directories(obj_gsp_sam
	PUBLIC
		${CMAKE_SOURCE_DIR}
		${OpenCV_INCLUDE_DIRS}
)
target_link_libraries(obj_gsp_sam
	PUBLIC
		${OpenCV_LIBS}
		onnxruntime::onnxruntime
		onnxruntime::onnxruntime_lora
		onnxruntime::onnxruntime_mlas
		onnxruntime::onnxruntime_util
)

add_library(obj_gsp_core STATIC
	${OBJ_GSP_DEBUGGER_SOURCES}
	${OBJ_GSP_FEATURE_SOURCES}
	${OBJ_GSP_MESH_SOURCES}
	${OBJ_GSP_STITCHING_SOURCES}
	${OBJ_GSP_UTIL_SOURCES}
)

target_include_directories(obj_gsp_core
	PUBLIC
		${CMAKE_SOURCE_DIR}
		${OpenCV_INCLUDE_DIRS}
)

target_link_libraries(obj_gsp_core
	PUBLIC
		obj_gsp_sam
		${OpenCV_LIBS}
		Eigen3::Eigen
		unofficial::vlfeat::vl
		gflags::gflags
)

add_executable(main main.cpp)
target_link_libraries(main PRIVATE obj_gsp_core)

option(OBJ_GSP_BUILD_SAM_TEST "Build SAM standalone test executable" OFF)

if(OBJ_GSP_BUILD_SAM_TEST)
	add_executable(sam_test ${OBJ_GSP_SAM_TEST_SOURCES})
	target_link_libraries(sam_test
		PRIVATE
			obj_gsp_sam
			${OpenCV_LIBS}
			gflags::gflags
	)
endif()
