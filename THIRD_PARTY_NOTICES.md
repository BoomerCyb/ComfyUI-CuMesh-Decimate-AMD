# Third-party notices

This node uses the bundled CuMesh HIP backend after it is built and installed into the existing ROCm Python environment.

## CuMesh

- Project: https://github.com/visualbruno/CuMesh
- Copyright: Copyright (c) 2025 Jianfeng XIANG
- License: MIT

The full upstream license notice is reproduced in `LICENSE` (section: CUMESH-MIT.txt).
Modified CuMesh source is bundled in `CuMesh-HIP/`. Build and install it using `README.md`; the legacy wheel-search installer is disabled.

## Bundled HIP backend

CuMesh sources originate from VisualBruno/CuMesh commit d10e54c30ddd03d11472c1431693f985501c7966. The backend retains its MIT license, third-party Eigen, cubvh and xatlas source notices, and AMD hipCUB 4.7.0, rocPRIM 4.8.0 and rocThrust 4.7.0 headers (from the ROCm 10.2.0 SDK) with their license files under CuMesh-HIP/third_party/amd; builds use the ROCm SDK's own copies of these headers when present. These components retain their own terms; see the license files under CuMesh-HIP/.
