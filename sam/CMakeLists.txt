add_library(samLib STATIC
    sam.cpp
)

target_include_directories(samLib PUBLIC
    ${OpenCV_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
)


target_link_libraries(samLib PUBLIC
    ${OpenCV_LIBS}
    onnxruntime::onnxruntime
    Eigen3::Eigen
    unofficial::vlfeat::vl
    gflags::gflags
    debuggerLib
)