# Comfy-RVC
ComfyUI custom nodes for RVC related inference and image generation

## Troubleshooting Dependencies

### `No module named 'monotonic_align'`

The `monotonic-align` package on PyPI has a version mismatch that causes pip to reject it. Install it directly from the tarball URL:

```bash
pip install "monotonic-align @ https://files.pythonhosted.org/packages/2e/fc/814cbd78dd57880267355179ef74ba24d12daeb68776221f58072ac70643/monotonic_align-1.0.0.tar.gz" --no-build-isolation
```

### Models location

Place your `.pth` model files in `ComfyUI/models/RVC/` and `.index` files in `ComfyUI/models/RVC/.index/`. Press **R** in ComfyUI to refresh the model list without restarting.
