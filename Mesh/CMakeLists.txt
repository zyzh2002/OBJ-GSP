add_library(meshLib
    MeshGrid.cpp
    MeshOptimization.cpp
)

target_include_directories(meshLib PUBLIC
    ${OpenCV_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
)


target_link_libraries(meshLib PUBLIC
    ${OpenCV_LIBS}
    onnxruntime::onnxruntime
    Eigen3::Eigen
    unofficial::vlfeat::vl
    gflags::gflags
    debuggerLib
)