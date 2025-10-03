# Xiaozhi AI OGG Batch Converter

This script is an OGG batch conversion tool that supports converting input audio files to OGG format usable by Xiaozhi
Implemented based on Python third-party library `ffmpeg-python`
Supports mutual conversion between OGG and audio, loudness adjustment and other functions

# Create and activate virtual environment

```bash
# Create virtual environment
python -m venv venv
# Activate virtual environment
source venv/bin/activate # Mac/Linux
venv\Scripts\activate # Windows
```

# Install dependencies

Please execute in virtual environment

```bash
pip install ffmpeg-python
```

# Run script
```bash
python xiaozhi_ogg_converter.py
