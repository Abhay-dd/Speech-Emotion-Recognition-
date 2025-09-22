#Speech Emotion Recognition (SER) Chatbot
This project is a Speech Emotion Recognition (SER) chatbot that analyzes audio input to detect one of 8 emotions (neutral, calm, happy, sad, angry, fearful, disgust, surprised) and provides a contextual response. The model is fine-tuned on the RAVDESS dataset using HuBERT from Hugging Face. The chatbot uses a backend script for real-time audio recording (via microphone), emotion prediction, speech-to-text transcription, and response generation.
Features

Records 4-second audio clips from the microphone.
Predicts emotions using a fine-tuned HuBERT model.
Transcribes speech to text using Google Speech Recognition.
Generates simple responses based on the detected emotion (e.g., "You sound excited!" for happy).
No frontend (console-based for simplicity).

Prerequisites

Python 3.9+
A microphone for audio input.
The RAVDESS dataset (download instructions below).
Internet access for speech-to-text (Google API).

Installation

Clone the repository:
bashgit clone <your-repo-url>
cd SER_Project

Create and activate a virtual environment:
bashpython -m venv .venv
source .venv/bin/activate  # On macOS/Linux
# or .venv\Scripts\activate on Windows

Install the required libraries from requirements.txt:
bashpip install -r requirements.txt


Required Libraries (requirements.txt)
The following libraries are used (installed via requirements.txt):

transformers==4.44.2: For loading and using the HuBERT model and feature extractor.
torch==2.4.1: PyTorch for model inference.
SpeechRecognition==3.14.3: For speech-to-text transcription using Google API.
pyaudio==0.2.14: For microphone audio input.
datasets==3.0.0: For dataset loading during training.
soundfile==0.12.1: For reading/writing audio files.
librosa==0.10.2.post1: For audio processing utilities.
evaluate==0.4.3: For accuracy metrics during training.
accelerate==1.0.0: For optimized training.
scikit-learn==1.5.1: For train-test splitting in dataset preparation.
numpy==1.26.4: For numerical operations.

Note: pyaudio may require additional setup on macOS (install portaudio via brew install portaudio if build errors occur).
Dataset

The project uses the RAVDESS (Ryerson Audio-Visual Database of Emotional Speech and Song) dataset, specifically the speech portion (~1.2 GB ZIP, ~11,520 speech files from 24 actors, 8 emotions).
Link: Download Audio_Speech_Actors_01-24.zip from Zenodo or Kaggle.
Extraction: Extract the ZIP to data/raw_ravdess/ in the project directory. The structure should be data/raw_ravdess/Actor_01/ to Actor_24/, with files like 03-01-01-01-01-01-01.wav (speech, vocal channel 01).

Note: Do not upload the dataset to GitHub (it's large and publicly available). Users should download it separately.
Usage

Prepare the Dataset:
bashpython prepare_dataset.py

This filters speech files (03-01-), maps to emotions, and splits into data/train/ (~9,216 files) and data/test/ (~2,304 files), organized by emotion.


Train the Model:
bashpython train_ser.py

Fine-tunes HuBERT for 3 epochs (~2-3 hours on CPU, ~20-30 min on GPU).
Saves the model to finetuned_ser_ravdess/ (expected accuracy ~75-85% on test set).


Run the Chatbot:
bashpython main.py

Press Enter to record 4 seconds of audio.
Outputs detected emotion, transcribed text, and response in the console.
Type 'quit' to exit.



Project Structure

prepare_dataset.py: Processes the RAVDESS dataset into train/test splits.
train_ser.py: Fine-tunes the HuBERT model on the dataset.
main.py: Backend script for real-time audio recording, emotion prediction, speech-to-text, and response generation.
requirements.txt: List of dependencies for installation.
README.md: This file (project documentation).
.gitignore: Excludes data folders, model outputs, and virtual environment.
data/: Contains raw and processed dataset (not uploaded; download separately).
finetuned_ser_ravdess/: Contains trained model files (not uploaded; train locally).

Note: Only code files are uploaded to GitHub. Download the dataset and train the model locally.
Notes

Training requires a GPU for efficiency.
The chatbot uses Google Speech Recognition, which requires internet access.
For microphone issues, install portaudio via Homebrew on macOS.
The urllib3 warning can be ignored or fixed by upgrading urllib3 and installing OpenSSL via brew install openssl.

If you encounter errors, check the virtual environment and dependencies. For help, open an issue on the GitHub repo.

What to Upload to GitHub

prepare_dataset.py (copy from previous responses).
train_ser.py (copy from previous responses).
main.py (the backend-only version you provided).
requirements.txt (as listed above).
README.md (the content I provided above).
.gitignore (from previous responses, to exclude data/, finetuned_ser_ravdess/, .venv/, etc.).

Do not upload:

data/ (large dataset).
finetuned_ser_ravdess/ (large model files).
.venv/ (virtual environment).
temp_audio.wav (temporary file).

How to Upload to GitHub

Create a repository on GitHub (e.g., "ser-chatbot").
Initialize Git in your project directory:
bashcd /Users/abhaydileep/Desktop/SER_Project
git init
git add .
git commit -m "Initial commit for SER Chatbot"
git remote add origin https://github.com/<your-username>/ser-chatbot.git
git push -u origin main

Add a license (e.g., MIT) and update the repo description on GitHub.

This setup allows anyone to see and use the code by downloading the dataset, installing libraries, preparing data, training the model, and running the chatbot. Let me know if you need help with any part! (It’s 12:49 AM IST, September 23, 2025—proceed as needed.)
