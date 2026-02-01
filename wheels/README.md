# Pre-built Wheels

This directory contains pre-built Python wheel files for dependencies that may be difficult to install from source.

## DeepSpeed Wheel

The included DeepSpeed wheel was built with:
- PyTorch 2.5.1
- CUDA 12.1
- Python 3.10.9

### Installation

```bash
pip install deepspeed-0.16.1+unknown-cp310-cp310-win_amd64.whl
```

### Compatibility

This wheel is built for Windows with the specific versions listed above. If you have a different setup, you may need to build your own wheel.
