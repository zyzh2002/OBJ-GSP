add_library(featureLib STATIC
    FeatureController.cpp
    ImageData.cpp
    MultiImage.cpp
)

target_include_directories(featureLib PUBLIC
    ${OpenCV_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
)

target_link_libraries(featureLib PUBLIC
    ${OpenCV_LIBS}
    onnxruntime::onnxruntime
    Eigen3::Eigen
    unofficial::vlfeat::vl
    gflags::gflags
    debuggerLib
)