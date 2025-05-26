# AI-Story-Teller
# AI-Story-Teller: Visual & Audio Narrative Generator

## Project Overview

The AI-Story-Teller is an innovative project that leverages the power of Artificial Intelligence to transform simple text prompts into engaging multimedia stories. It generates coherent narratives, visualizes key scenes with AI-generated images, narrates the story using text-to-speech, and finally assembles everything into a compelling video.

This project is particularly well-suited for running on **Google Colab**, taking advantage of its free GPU access for computationally intensive tasks like image generation.

## Features

* **Story Generation:** Generates multi-segment stories based on a user-provided prompt, maintaining coherence across segments.
* **Image Generation:** Creates unique visual scenes corresponding to parts of the generated story using state-of-the-art diffusion models.
* **Audio Narration:** Converts story text into natural-sounding speech.
* **Video Assembly:** Combines the generated images and audio narration into a synchronized video file.
* **Gradio Interface:** Provides a simple, user-friendly web interface for easy interaction directly within your browser.

## How It Works (The Pipeline)

The AI-Story-Teller operates through a sequential pipeline:

1.  **Story Generation (`StoryGenerator`):**
    * Takes your initial prompt.
    * Uses a pre-trained Large Language Model (LLM) like GPT-2 to generate story segments.
    * Crucially, it feeds the *entire story generated so far* back into the LLM as context for the next segment, significantly improving narrative coherence.
2.  **Image Generation (`ImageGenerator`):**
    * For each story segment, it extracts a key sentence (typically the first).
    * Feeds this sentence as a prompt to a Stable Diffusion model to create a corresponding image.
3.  **Audio Generation (`AudioGenerator`):**
    * Converts the full text of each story segment into an audio file using a Text-to-Speech (TTS) model (e.g., Coqui-TTS).
4.  **Multimedia Assembly (`MultimediaAssembler`):**
    * Combines each generated image with its corresponding audio narration.
    * Synchronizes the duration of the image display with the length of the audio.
    * Renders the sequence of image-audio clips into a single `.mp4` video file using `moviepy`.

## Technologies Used

* **Python 3.x**
* **Hugging Face Transformers:** For text generation (GPT-2 pipeline).
* **Hugging Face Diffusers:** For image generation (Stable Diffusion).
* **Coqui-TTS:** For advanced Text-to-Speech synthesis.
* **MoviePy:** For video editing and assembly.
* **Pillow (PIL):** For image manipulation (e.g., creating placeholders).
* **Pydub:** For audio manipulation (often a dependency of moviepy/TTS).
* **Gradio:** For creating the user-friendly web interface.
* **Numpy:** Core dependency for many scientific computing libraries.

## Setup and Installation (Google Colab Guide)

This project is optimized for Google Colab due to its free GPU access.

**Step 1: Open Google Colab and Configure Runtime**

1.  Go to [Google Colab](https://colab.research.google.com/).
2.  Click "File" -> "New notebook".
3.  Click "Runtime" -> "Change runtime type".
4.  Under "Hardware accelerator", select **"GPU"**.
5.  Click "Save".
6.  **Crucially, restart your session:** Click "Runtime" -> "Restart session". This ensures a clean environment.

**Step 2: Run Installation Cell**

Copy and paste the following code into the **first cell** of your Colab notebook and run it. This will install all necessary system tools and Python libraries. This step can take a few minutes as it downloads many packages.

```python
# --- Installation Cell ---
!apt-get update
!apt-get install -y ffmpeg
!pip install moviepy

# Ensure numpy version compatibility for coqui_tts and other libraries
print("\n--- Cleaning up previous installations ---")
!pip uninstall coqui-tts -y
!pip uninstall numpy -y
!pip uninstall TTS -y
!pip uninstall gradio -y

# Install a specific numpy version known to be compatible with many TTS models
print("\n--- Installing numpy==1.23.5 ---")
!pip install numpy==1.23.5

# Install coqui_tts and other core libraries
print("\n--- Installing coqui_tts and other core libraries ---")
!pip install --upgrade setuptools pip
!pip install coqui_tts

# Install the rest of the required Python libraries
!pip install transformers accelerate diffusers pydub Pillow gradio

# --- Verification Step (Optional, but highly recommended for debugging) ---
print("\n--- Verifying key library installations ---")
try:
    from TTS.api import TTS
    print("Successfully imported 'TTS' from 'TTS.api'")
except Exception as e:
    print(f"Failed to import 'TTS': {e}")
    print("WARNING: coqui_tts might not be fully installed or accessible. Check for errors above.")

try:
    import gradio as gr
    print("Successfully imported 'gradio'")
except Exception as e:
    print(f"Failed to import 'gradio': {e}")
    print("WARNING: gradio might not be fully installed or accessible. Check for errors above.")

print("\nAll initial installations and checks complete. Proceed to the next cell.")
