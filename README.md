# Ai_Voice_Cloner_-_Text_To_Speech_Generator_Urdu_
# ✅ Coqui TTS (for English custom voice)
!pip install TTS==0.22.0

# ✅ Urdu TTS (Google TTS)
!pip install gtts

# ✅ For converting MP3 → WAV
!pip install pydub

# ✅ For web app UI
!pip install gradio

# ✅ Required system tool for pydub
!apt install ffmpeg -y
##
from TTS.api import TTS
from gtts import gTTS
from pydub import AudioSegment
import gradio as gr
import os

# ✅ Built-in model (English, cloned or generic voice)
tts_en = TTS(model_name="tts_models/en/ljspeech/tacotron2-DDC", progress_bar=False, gpu=False)
##
def generate_tts(text, language):
    if language == "English":
        output_path = "output_en.wav"
        tts_en.tts_to_file(text=text, file_path=output_path)
        return output_path
    else:
        # Urdu TTS using gTTS
        tts_urdu = gTTS(text=text, lang='ur')
        tts_urdu.save("temp_urdu.mp3")
        sound = AudioSegment.from_mp3("temp_urdu.mp3")
        sound.export("output_ur.wav", format="wav")
        return "output_ur.wav"
 ##
 import gradio as gr  

interface = gr.Interface(
    fn=generate_tts,
    inputs=[
        gr.Textbox(lines=4, label="Enter Text (Urdu or English)"),
        gr.Radio(choices=["English", "Urdu"], label="Select Language") 
    ],
    outputs=gr.Audio(type="filepath", label="Generated Audio"),
    title="Custom AI Voice Cloner & Urdu TTS Generator",
    description="Enter English for cloned voice or Urdu for natural TTS"
)

interface.launch(share=True)
