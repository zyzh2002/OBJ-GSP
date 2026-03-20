add_library(stitchingLib STATIC
    APAP_Stitching.cpp
    NISwGSP_Stitching.cpp
    Parameter.cpp
)

target_include_directories(stitchingLib PUBLIC
    ${OpenCV_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
)


target_link_libraries(stitchingLib PUBLIC
    ${OpenCV_LIBS}
    onnxruntime::onnxruntime
    Eigen3::Eigen
    unofficial::vlfeat::vl
    gflags::gflags
    debuggerLib
    meshLib
)