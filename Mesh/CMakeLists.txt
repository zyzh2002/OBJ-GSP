# Mesh2D.cpp is directly included by MeshGrid.cpp, so it must not be
# compiled as a separate translation unit to avoid duplicate symbols.
set(OBJ_GSP_MESH_SOURCES
    ${CMAKE_CURRENT_LIST_DIR}/MeshGrid.cpp
    ${CMAKE_CURRENT_LIST_DIR}/MeshOptimization.cpp
    PARENT_SCOPE
)