# OBJ-GSP

Implementation of the AAAI 2025 paper *Object-level Geometric Structure Preserving for Natural Image Stitching* (Unofficial Port).

This README is rewritten for this fork based on commit history since `98828c91b6db825ec3ec4aad698e03653f319e3a`, with emphasis on required engineering changes.

## Required Fork Changes (since `98828c9`)

1. Migrated from manual `.sln` management to unified CMake builds.
   - Added top-level and per-module `CMakeLists.txt`.
   - Split `Debugger/Feature/Mesh/Stitching/Util/sam` into static libraries and linked them centrally.
   - Why necessary: better maintainability and reproducible Linux/WSL/CI builds.

2. Added CMake presets and vcpkg manifest workflow.
   - Added `CMakePresets.json`, `CMakeUserPresets.json`, `vcpkg.json`, and `vcpkg-configuration.json`.
   - Why necessary: standardized setup and easier onboarding on new machines.

3. Updated dependency strategy.
   - vcpkg now manages `gflags`, `vlfeat`, and `eigen3`.
   - `eigen3` is pinned to `3.4.0` via override.
   - `onnxruntime` is no longer pulled through manifest and should be provided as a local/system CMake package.
   - Why necessary: improves dependency resolution reliability across platforms.

4. Refactored linking and enabled OpenMP.
   - Added `find_package(OpenMP REQUIRED)` in root CMake.
   - Submodules now use explicit `target_link_libraries` dependencies.
   - Why necessary: avoids hidden linker issues and ensures parallel-related paths build correctly.

5. Cross-platform cleanup (removed Windows-specific artifacts).
   - Replaced directory creation with `mkdir(path, 0755)` in `Stitching/Parameter.cpp`.
   - Removed `VlFeat.props` and `vl.dll` from repository.
   - Removed bundled `dirent.h` and switched to system header usage.
   - Why necessary: native Linux build compatibility without platform-bound binaries.

6. Header/include cleanup and formatting fixes.
   - Adjusted include paths and Eigen geometry-related headers.
   - Why necessary: reduces compiler-dependent include/symbol issues.

## Environment and Dependencies

- CMake >= 3.22
- Ninja
- GCC/G++ (current preset defaults to `gcc-9` / `g++-9`, not guranteed to work with newer versions due to potential C++ grammar changes)
- OpenCV (4.x recommended)
- ONNX Runtime (must be discoverable by `find_package(onnxruntime CONFIG REQUIRED)`)
- vcpkg (for `gflags`, `vlfeat`, `eigen3`)

## Build (Recommended)

1. Set vcpkg root:

```bash
export VCPKG_ROOT=/root/vcpkg
```

2. Configure:

```bash
cmake --preset vcpkg
```

3. Build:

```bash
cmake --build build -j
```

4. Run:

```bash
./build/main
```

If CMake cannot find OpenCV or ONNX Runtime, compile and install them manually with the `.pc` files or CMake config files properly set up.

## Datasets

1. StitchBench
   - Sample pair in `./input-data/AANAP-01_skyline`.
   - Full dataset: https://huggingface.co/datasets/RussRobin/StitchBench

2. Aerial234
   - Dataset: https://huggingface.co/datasets/RussRobin/Aerial234
   - Related work: https://www.sciencedirect.com/science/article/pii/S1047320323000858

## SAM Notes

The repository includes `sam/` and `sam.ipynb`. For SAM-related reproduction, ensure ONNX model paths and runtime I/O expectations match your local setup.

## Citation

```bibtex
@inproceedings{cai2025object,
  title={Object-level geometric structure preserving for natural image stitching},
  author={Cai, Wenxiao and Yang, Wankou},
  booktitle={Proceedings of the AAAI Conference on Artificial Intelligence},
  volume={39},
  number={2},
  pages={1926--1934},
  year={2025}
}
```

```bibtex
@article{cai2023uav,
  title={UAV image stitching by estimating orthograph with RGB cameras},
  author={Cai, Wenxiao and Du, Songlin and Yang, Wankou},
  journal={Journal of Visual Communication and Image Representation},
  volume={94},
  pages={103835},
  year={2023},
  publisher={Elsevier}
}
```

If you have questions, feel free to open an issue.
