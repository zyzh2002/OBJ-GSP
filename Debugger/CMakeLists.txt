add_library(debuggerLib STATIC
    ColorMap.cpp
    ErrorController.cpp
    ImageDebugger.cpp
    TimeCalculator.cpp
)

target_include_directories(debuggerLib PUBLIC
    ${OpenCV_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
)

target_link_libraries(debuggerLib PUBLIC
    ${OpenCV_LIBS}
    OpenMP::OpenMP_CXX
    onnxruntime::onnxruntime
    Eigen3::Eigen
    unofficial::vlfeat::vl
    gflags::gflags
)