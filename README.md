
# Naija Pidgin ASR Data Preparation

This repository contains a Python pipeline designed to process the **Universal Dependencies Naija-NSC (Nigerian Pidgin)** corpus. It automates the extraction of audio, alignment of timestamps, and generation of sentence-level audio clips suitable for training Automatic Speech Recognition (ASR) models or Text-to-Speech (TTS) systems.

## 🎯 Overview

The script performs the following operations:
1.  **Parses CoNLL-U files**: Extracts metadata, transcriptions (`text_ortho`), and alignment timestamps from the raw dataset.
2.  **Filters Data**: Cleans the dataset by removing:
    *   Sentences containing ellipses (`...`).
    *   Sentences with fewer than 4 words.
3.  **Parallel Downloading**: efficient multi-threaded downloading of the source MP3 files.
4.  **Audio Processing**:
    *   Converts source MP3s to WAV format using `ffmpeg`.
    *   **Slices** the audio into individual sentence clips based on milliseconds timestamps.
5.  **Manifest Generation**: Creates a clean `transcripts.csv` file mapping text to audio.
6.  **Packaging**: Zips the resulting dataset for easy export.

## 📂 Data Source

This project utilizes data from the [Universal Dependencies Naija NSC](https://github.com/UniversalDependencies/UD_Naija-NSC).
*   **Language:** Nigerian Pidgin (pcm)
*   **File:** `pcm_nsc-ud-train.conllu`

## 🛠️ Requirements

This script is designed to run in a **Google Colab** or Linux environment (due to `wget` and `ffmpeg` dependencies).

**System Dependencies:**
*   `ffmpeg` (Required for audio conversion)
*   `wget`

**Python Libraries:**
*   `pydub`
*   `pandas`
*   `tqdm`
*   `requests`

## 🚀 Usage

### Running on Google Colab (Recommended)
1.  Upload the script/notebook to Google Colab.
2.  Run the cells sequentially.
3.  The script will handle library installation and mounting Google Drive.

### Running Locally
If running locally, ensure you have `ffmpeg` installed on your system path and remove the Google Colab specific commands (lines starting with `!` or `drive.mount`).

```bash
pip install pydub pandas tqdm requests
python main.py
```

## 📊 Output Structure

After running the pipeline, the file structure will look like this:

```text
.
├── full_mp3s/           # Raw downloaded audio files
├── full_wavs/           # Converted WAV files
├── clips/               # Sliced sentence-level audio clips (e.g., pidgin_0001.wav)
├── transcripts.csv      # Text transcriptions (No header, single column)
├── clips_python.zip     # Zipped archive of all clips
└── pcm_nsc-ud-train.conllu
```

## ⚙️ Configuration

You can adjust the filtering logic in the script to change the dataset size:

```python
filtered_data = [
    item for item in prepared_data
    if "..." not in item['transcription'] and \
       len(item['transcription'].split()) >= 4  # Adjust word count threshold here
]
```

## 🤝 Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## 📜 License

This code is open source. The underlying data belongs to the [Universal Dependencies project](https://universaldependencies.org/) and implies their specific licensing terms (CC BY-SA 4.0).
