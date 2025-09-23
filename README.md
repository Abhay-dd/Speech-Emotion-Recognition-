Speech Emotion Recognition (SER) Chatbot

How to Use the SER Chatbot Project (ZIP File Guide)

Speech Emotion Recognition (SER) Chatbot project! This guide provides everything you need to set up, run, and troubleshoot the project. The ZIP contains the core code files only (no dataset or trained model, as they are large and must be downloaded separately). The project analyzes 4-second audio recordings to detect emotions (neutral, calm, happy, sad, angry, fearful, disgust, surprised) using a fine-tuned HuBERT model trained on the RAVDESS dataset, transcribes speech to text, and generates responses.

Project Overview:

Backend-Only: Console-based for simplicity (no web interface).
Dataset: RAVDESS speech audio (~11,520 files from 24 actors, 8 emotions).
Accuracy: ~75-85% on test set after training.
Time: Dataset preparation ~5 minutes, training ~2-3 hours on CPU.

1. Prerequisites
   OS: macOS, Linux, or Windows.
   Python: 3.9+ (check with python --version).
   Microphone: For real-time audio input.
   Internet: For speech-to-text (Google API) and downloads.
   Tools: Git (optional), Homebrew (for macOS dependencies).
2. Installation
   Extract the ZIP:
     Unzip the file to a folder (e.g., SER_Project).
     Navigate to it:
     #cd SER_Project
   Create a Virtual Environment (Recommended):
     Create:
     #python -m venv .venv
     Activate:
     macOS/Linux:  #source .venv/bin/activate
     Windows:  #.venv\Scripts\activate
   Install Libraries:
     Install pip upgrades:  #pip install --upgrade pip
     Install from requirements.txt:  #pip install -r requirements.txt
   macOS Note: If pyaudio fails, install portaudio:  #brew install portaudio
     pip install pyaudio
3. Download the Dataset
   The dataset is not included in the ZIP (to keep it small). Download and extract it manually.
   Download Link:
   Primary : https://zenodo.org/records/1188976/files/Audio_Speech_Actors_01-24.zip
   Alternative : https://www.kaggle.com/datasets/uwrfkaggler/ravdess-emotional-speech-audio

   Extract:
     Unzip to data/raw_ravdess/ in the project folder : unzip Audio_Speech_Actors_01-24.zip -d data/raw_ravdess/
   #mv data/raw_ravdess/*/Actor_* data/raw_ravdess/
   Structure: data/raw_ravdess/Actor_01/ to Actor_24/, with files like 03-01-01-01-01-01-01.wav (speech, vocal channel 01).
   Verify : #ls data/raw_ravdess/
  #ls data/raw_ravdess/Actor_01/ | head -5
  #find data/raw_ravdess/ -name "03-01-*.wav" | wc -l

  Expect: 24 actor folders, ~144 speech files per actor, ~11,520 total 03-01-*.wav files.
  
4. Usage
  Prepare Dataset:  #python prepare_dataset.py
  Output: Creates data/train/ (~9,216 files) and data/test/ (~2,304 files), organized by emotion (e.g., data/train/happy/).
  Time: ~5 minutes.

  Train the Model :  #python train_ser.py
  
  Output: Fine-tunes HuBERT for 3 epochs, saves to finetuned_ser_ravdess/. Expected accuracy ~75-85% on test set.
  Time: ~2-3 hours on CPU, ~20-30 minutes on GPU.
  Note: Requires internet for initial model download.
  
  Run the Chatbot :  #python main.py

  Interaction:
    Prompt: SER Chatbot: Press Enter to record audio (4 seconds). Type 'quit' to exit.
    Press Enter to record (speak into microphone).


    Sample Output:
    Recording...
Recording finished.

Detected emotion: happy

You said: I am happy today

You sound excited! What's got you in such a great mood?

Type quit or control+c to exit.

Time: Real-time (~4 seconds per recording).

5. Troubleshooting
   "ModuleNotFoundError" (e.g., SpeechRecognition)
   Reinstall: #pip install SpeechRecognition==3.14.3.
   Import: Ensure import speech_recognition as sr in main.py.

   Microphone Not Working : 
   Install pyaudio: #pip install pyaudio.
   macOS: brew install portaudio then reinstall pyaudio.
   Check permissions in System Preferences > Security & Privacy > Microphone.

   "FileNotFoundError" for Dataset : 
   Download/extract RAVDESS to data/raw_ravdess/ (see Step 3).

   Training Fails : 
   Ensure dataset is prepared (ls data/train/).
   Use GPU if available (check python -c "import torch; print(torch.cuda.is_available())").
   Reduce batch size in train_ser.py if memory error.

   "urllib3 NotOpenSSLWarning" : 
   Ignore or fix: pip install --upgrade urllib3; brew install openssl.

   No Output from main.py :
   Verify model: ls finetuned_ser_ravdess/ (rerun train_ser.py if missing).
   Test microphone: python -c "import pyaudio; p = pyaudio.PyAudio(); print(p.get_device_count())".

6. Additional Notes
   Accuracy: ~75-85% on test set; depends on training data quality.
   Customization: Modify main.py to change recording duration or response logic.
   Extensions: Add a frontend (e.g., Gradio/Streamlit) or deploy to Hugging Face Spaces.
   
