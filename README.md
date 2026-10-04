# ComfyUI-CuMesh-Decimate-AMD

## AMD / ROCm

The installer uses ComfyUI's Python and stops if setup fails. When installing through EZi, wait for the entire node group to complete before restarting.

This fork keeps the original node interface and adds native HIP support.
Use the ROCm PyTorch installation that runs ComfyUI and a matching HIP SDK.
The source does not select a card model or impose a gfx1201 target. Native
extensions target visible discrete AMD GPUs by default, excluding integrated GPUs
when a discrete GPU is available. Set PYTORCH_ROCM_ARCH to override the targets.
Integrated-only systems remain supported; PyTorch supplies the compiler flags.
An installed binary still needs to match its GPU target, Python and Torch runtime.
Hardware support depends on ROCm/PyTorch; validation here covers RX 9070 XT.

For manual installation, close ComfyUI and run `install_requirements.bat` to build/install the native components. ComfyUI-Easy-Install-AMD runs this automatically through its add-on menu.
For prerequisites and manual commands, see [COMFYUI_ROCM_BUILD_GUIDE.md](COMFYUI_ROCM_BUILD_GUIDE.md).

ComfyUI AMD installer: [BoomerCyb/ComfyUI-Easy-Install-AMD](https://github.com/BoomerCyb/ComfyUI-Easy-Install-AMD).


Standalone native ComfyUI `MESH -> MESH` geometry decimation using the
MIT-licensed [VisualBruno CuMesh](https://github.com/visualbruno/CuMesh) backend, ported to HIP in this fork.

## Node

`CuMesh - Geometry Decimate (GPU)`

The output contains only vertices, triangle faces, and optional newly computed smooth
normals. UVs, textures, colors, materials, tangents, and maps are deliberately discarded.

## Installation

### ComfyUI-Easy-Install-AMD

In [ComfyUI-Easy-Install-AMD](https://github.com/BoomerCyb/ComfyUI-Easy-Install-AMD), select **Easy Menu → Add-ons → BoomerCyb WTiVo AMD Nodes**. It downloads the nodes and runs their installers automatically. You do not need to run `install_requirements.bat` separately. Wait for all five nodes to finish; EZi restarts ComfyUI after the group completes successfully.

### Manual installation

1. Place this repository in `ComfyUI/custom_nodes/ComfyUI-CuMesh-Decimate-AMD`.
2. Close ComfyUI and run `install_requirements.bat` using ComfyUI's Python.
3. Restart ComfyUI after installation completes.

## Controls

- `target_faces`: requested maximum triangle count; accepts every integer from 1 upward.
- `initial_threshold`: CuMesh's starting collapse threshold. The upstream default is `1e-8`.
- `edge_length_weight`: discourages uneven edge lengths. Upstream default: `0.01`.
- `skinny_triangle_weight`: penalizes skinny triangles. Upstream default: `0.001`.
- `unload_models`: unload ComfyUI models before CuMesh allocates VRAM. Keep this enabled
  on an 8 GB GPU. ComfyUI reloads a model automatically if a later node needs it.
- `output_smooth_normals`: computes fresh smooth vertex normals after decimation.
- `verbose`: prints CuMesh simplification progress.

CuMesh performs parallel edge collapses, so the final count can be slightly below the
requested target. A very aggressive reduction still removes real geometric detail; no
simplifier can preserve a 50-million-face surface identically at 100,000 faces.

## Suggested starting settings

Use the upstream defaults first:

```text
initial_threshold       0.00000001
edge_length_weight      0.01
skinny_triangle_weight  0.001
unload_models           true
output_smooth_normals   true
verbose                 true
```

If thin triangles remain, increase `skinny_triangle_weight` gradually (for example,
`0.002`, then `0.005`). Large changes can alter the result, so compare visually.

## License

This wrapper is MIT licensed. CuMesh is a separate MIT-licensed dependency; see
`THIRD_PARTY_NOTICES.md` and `licenses/CUMESH-MIT.txt`.

## AMD Edition Changes - 2026-10-03

CuMesh uses HIP with hipCUB/rocPRIM, corrected sorting/header compatibility,
and bounded or flat memory transfers for large meshes. The native backend is
shared with Mesh Quad Reconstruct.

- Uses ComfyUI's Python and reports installation failures before restarting.
- Builds native HIP extensions for the active ROCm environment; matching HIP SDK and Visual Studio C++ Build Tools are required.
- Supports group installation through [ComfyUI-Easy-Install-AMD](https://github.com/BoomerCyb/ComfyUI-Easy-Install-AMD).

Original node by [Mstafa-awad / MostAadTech](https://github.com/Mstafa-awad). AMD fork maintained by [BoomerCyb](https://github.com/BoomerCyb). Original license and third-party credits are retained.
