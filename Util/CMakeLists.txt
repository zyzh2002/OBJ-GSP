add_library(utilLib STATIC
    Blending.cpp
    EdgeDetection.cpp
    InputParser.cpp
    Statistics.cpp
    Thin.cpp
    Transform.cpp
)

target_include_directories(utilLib PUBLIC
    ${OpenCV_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
)


target_link_libraries(utilLib PUBLIC
    ${OpenCV_LIBS}
    onnxruntime::onnxruntime
    Eigen3::Eigen
    unofficial::vlfeat::vl
    gflags::gflags
    debuggerLib
)