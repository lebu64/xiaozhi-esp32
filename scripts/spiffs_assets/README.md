# SPIFFS Assets Builder

This script is used to build SPIFFS resource partitions for ESP32 projects, packaging various resource files into formats usable on devices.

## Features

- Processes wake network model (WakeNet Model)
- Integrates text font files
- Processes emoji image collections
- Automatically generates resource index files
- Packages and generates final `assets.bin` file

## Requirements

- Python 3.6+
- Related resource files

## Usage

### Basic Syntax

```bash
./build.py --wakenet_model <wakenet_model_dir> \
    --text_font <text_font_file> \
    --emoji_collection <emoji_collection_dir>
```

### Parameter Description

| Parameter            | Type           | Required | Description                           |
|----------------------|----------------|----------|---------------------------------------|
| `--wakenet_model`    | Directory path | No       | Wake network model directory path     |
| `--text_font`        | File path      | No       | Text font file path                   |
| `--emoji_collection` | Directory path | No       | Emoji image collection directory path |

### Usage Examples

```bash
# Complete parameter example
./build.py \
    --wakenet_model ../../managed_components/espressif__esp-sr/model/wakenet_model/wn9_nihaoxiaozhi_tts \
    --text_font ../../components/xiaozhi-fonts/build/font_puhui_common_20_4.bin \
    --emoji_collection ../../components/xiaozhi-fonts/build/emojis_64/

# Process only font files
./build.py --text_font ../../components/xiaozhi-fonts/build/font_puhui_common_20_4.bin

# Process only emojis
./build.py --emoji_collection ../../components/xiaozhi-fonts/build/emojis_64/
```

## Workflow

1. **Create build directory structure**
   - `build/` - Main build directory
   - `build/assets/` - Resource files directory
   - `build/output/` - Output files directory

2. **Process wake network model**
   - Copy model files to build directory
   - Use `pack_model.py` to generate `srmodels.bin`
   - Copy generated model files to resource directory

3. **Process text fonts**
   - Copy font files to resource directory
   - Supports `.bin` format font files

4. **Process emoji collections**
   - Scan image files in specified directory
   - Supports `.png` and `.gif` formats
   - Automatically generates emoji index

5. **Generate configuration files**
   - `index.json` - Resource index file
   - `config.json` - Build configuration file

6. **Package final resources**
   - Use `spiffs_assets_gen.py` to generate `assets.bin`
   - Copy to build root directory

## Output Files

After build completion, the following files will be generated in the `build/` directory:

- `assets/` - All resource files
- `assets.bin` - Final SPIFFS resource file
- `config.json` - Build configuration
- `output/` - Intermediate output files

## Supported Resource Formats

- **Model files**: `.bin` (processed via pack_model.py)
- **Font files**: `.bin`
- **Image files**: `.png`, `.gif`
- **Configuration files**: `.json`

## Error Handling

The script includes comprehensive error handling mechanisms:

- Checks if source files/directories exist
- Verifies subprocess execution results
- Provides detailed error information and warnings

## Notes

1. Ensure all dependent Python scripts are in the same directory
2. Use absolute paths or paths relative to the script directory for resource files
3. Build process will clean previous build files
4. Generated `assets.bin` file size is limited by SPIFFS partition size
