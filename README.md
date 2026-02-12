# Comfy-RVC
ComfyUI custom nodes for RVC related inference and image generation

## Troubleshooting Dependencies

### `No module named 'monotonic_align'`

The `monotonic-align` package on PyPI has a version mismatch that causes pip to reject it. Install it directly from the tarball URL:

```bash
pip install "monotonic-align @ https://files.pythonhosted.org/packages/2e/fc/814cbd78dd57880267355179ef74ba24d12daeb68776221f58072ac70643/monotonic_align-1.0.0.tar.gz" --no-build-isolation
```

### `No module named 'fairseq'`

The `fairseq` package does not provide pre-built wheels and fails to compile on Windows embedded Python due to missing `python3xx.lib` and `Python.h`. To install it:

1. **Create `python3xx.lib`** from the DLL (replace `312` with your Python version):
   ```bash
   # In a VS Developer Command Prompt:
   dumpbin /EXPORTS python312.dll > exports.def
   # Edit exports.def to proper .def format, then:
   lib /DEF:exports.def /OUT:libs/python312.lib /MACHINE:X64
   ```

2. **Download Python headers** from https://www.python.org/ftp/python/ matching your version and copy them to `python_embeded/include/`.

3. **Install fairseq without C extensions** (sufficient for RVC inference):
   ```bash
   git clone --depth 1 https://github.com/facebookresearch/fairseq.git /tmp/fairseq
   ```
   Edit `/tmp/fairseq/setup.py`: replace `extensions = [...]` block with `extensions = []` and `cmdclass = {"build_ext": ...}` with `cmdclass = {}`, then:
   ```bash
   pip install /tmp/fairseq --no-deps --no-build-isolation
   ```

4. **Install compatible hydra/omegaconf** (fairseq pins old versions incompatible with Python 3.12+):
   ```bash
   pip install "hydra-core>=1.3" "omegaconf>=2.3"
   ```

5. **Fix mutable dataclass defaults** in fairseq for Python 3.12+: replace all `field_name: SomeConfig = SomeConfig()` with `field_name: SomeConfig = field(default_factory=SomeConfig)` in fairseq's dataclass files (configs.py, transformer_config.py, etc).

### Models location

Place your `.pth` model files in `ComfyUI/models/RVC/` and `.index` files in `ComfyUI/models/RVC/.index/`. Press **R** in ComfyUI to refresh the model list without restarting.
