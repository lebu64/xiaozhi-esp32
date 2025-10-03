# P3 Audio Format Conversion and Playback Tools

This directory contains two Python scripts for processing P3 format audio files:

## 1. Audio Conversion Tool (convert_audio_to_p3.py)

Converts regular audio files to P3 format (4-byte header + Opus data packet stream structure) and performs loudness normalization.

### Usage

```bash
python convert_audio_to_p3.py <input audio file> <output P3 file> [-l LUFS] [-d]
```

Where the optional option `-l` is used to specify the target loudness for loudness normalization, default is -16 LUFS; optional option `-d` can disable loudness normalization.

If the input audio file meets any of the following conditions, it is recommended to use `-d` to disable loudness normalization:
- Audio is too short
- Audio has already been adjusted for loudness
- Audio comes from default TTS (the default loudness of Xiaozhi's current TTS is already -16 LUFS)

For example:
```bash
python convert_audio_to_p3.py input.mp3 output.p3
```

## 2. P3 Audio Playback Tool (play_p3.py)

Plays P3 format audio files.

### Features

- Decodes and plays P3 format audio files
- Applies fade-out effect when playback ends or user interrupts to avoid clipping
- Supports specifying files to play through command line parameters

### Usage

```bash
python play_p3.py <P3 file path>
```

For example:
```bash
python play_p3.py output.p3
```

## 3. Audio Conversion Back Tool (convert_p3_to_audio.py)

Converts P3 format back to regular audio files.

### Usage

```bash
python convert_p3_to_audio.py <input P3 file> <output audio file>
```

Output audio file needs to have an extension.

For example:
```bash
python convert_p3_to_audio.py input.p3 output.wav
```

## 4. Audio/P3 Batch Conversion Tool

A graphical tool that supports batch conversion of audio to P3, and P3 to audio

![](./img/img.png)

### Usage:
```bash
python batch_convert_gui.py
```

## Dependency Installation

Before using these scripts, please ensure the required Python libraries are installed:

```bash
pip install librosa opuslib numpy tqdm sounddevice pyloudnorm soundfile
```

Or use the provided requirements.txt file:

```bash
pip install -r requirements.txt
```

## P3 Format Description

P3 format is a simple streaming audio format with the following structure:
- Each audio frame consists of a 4-byte header and an Opus encoded data packet
- Header format: [1 byte type, 1 byte reserved, 2 bytes length]
- Sample rate fixed at 16000Hz, mono
- Each frame duration is 60ms
