# Day 23: Speech-to-Text with Whisper

## =Ì Objective
Master OpenAI's Whisper model for accurate, multilingual speech-to-text transcription, and learn how to integrate audio processing into your applications.

## <¯ Today's Exercise (10-15 minutes)

### Prerequisites
- Python 3.8+
- `pip install openai python-dotenv`
- OpenAI API key with Whisper access
- An audio file or ability to record audio

### Part 1: Understanding Whisper (2 min)

**What is Whisper?**

Whisper is OpenAI's robust speech recognition model that:
- Handles diverse accents, technical vocabulary, and background noise
- Supports 97 languages
- Can transcribe, translate, and detect language
- Works with various audio formats (mp3, mp4, mpeg, mpga, m4a, wav, webm)
- Has up to 25MB file size limit per request
- Supports both transcription and translation to English

**Why Whisper is Powerful**:
- **Multilingual**: Works with non-English languages automatically
- **Noise-Resistant**: Handles background noise well
- **Technical Terms**: Understands specialized vocabulary
- **No Training Required**: Off-the-shelf usage
- **Affordable**: Significantly cheaper than other services

**Key Features**:
1. **Transcription**: Convert speech to text (original language)
2. **Translation**: Convert speech to English text
3. **Language Detection**: Identify the language spoken
4. **Prompt Engineering**: Provide context for accuracy

### Part 2: Basic Whisper Implementation (5 min)

**Create `whisper_demo.py`**:

```python
import os
from openai import OpenAI
from pathlib import Path
from dotenv import load_dotenv

load_dotenv()

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

print("="*60)
print("WHISPER SPEECH-TO-TEXT EXAMPLES")
print("="*60)

audio_file_path = "sample_audio.mp3"

# Example 1: Basic transcription
if Path(audio_file_path).exists():
    with open(audio_file_path, "rb") as audio_file:
        transcript = client.audio.transcriptions.create(
            model="whisper-1",
            file=audio_file
        )
    print(f"Transcript:\n{transcript.text}\n")

# Example 2: Transcription with context
if Path(audio_file_path).exists():
    with open(audio_file_path, "rb") as audio_file:
        transcript = client.audio.transcriptions.create(
            model="whisper-1",
            file=audio_file,
            language="en",
            prompt="The speaker discusses AI and machine learning"
        )
    print(f"Improved Transcript:\n{transcript.text}\n")

# Example 3: Translation to English
if Path(audio_file_path).exists():
    with open(audio_file_path, "rb") as audio_file:
        translation = client.audio.translations.create(
            model="whisper-1",
            file=audio_file
        )
    print(f"English Translation:\n{translation.text}\n")

# Example 4: Language detection
if Path(audio_file_path).exists():
    with open(audio_file_path, "rb") as audio_file:
        transcript = client.audio.transcriptions.create(
            model="whisper-1",
            file=audio_file,
            response_format="verbose_json"
        )
    if hasattr(transcript, 'language'):
        print(f"Detected Language: {transcript.language}")

# Example 5: Batch processing
audio_files = ["audio1.mp3", "audio2.mp3", "audio3.mp3"]
for audio_file in audio_files:
    if Path(audio_file).exists():
        with open(audio_file, "rb") as f:
            transcript = client.audio.transcriptions.create(
                model="whisper-1",
                file=f
            )
        print(f"Transcribed {audio_file}")

print("Whisper demo complete!")
```

**Run it**:
```bash
python whisper_demo.py
```

### Part 3: Creating Test Audio (3 min)

**Option 1: Generate Using TTS** (from yesterday):

```bash
python tts_demo.py  # Creates test audio files
```

**Option 2: Record Your Own**:

```bash
# On macOS:
ffmpeg -f avfoundation -i ":0" -t 10 output.wav

# On Linux:
ffmpeg -f pulse -i default -t 10 output.wav
```

## =¡ Key Takeaways

- **Whisper is multilingual** - supports 97 languages automatically
- **Context improves accuracy** - use prompts for domain-specific content
- **Translation feature** - convert any language to English
- **Noise-resistant** - handles background noise well
- **Cost-effective** - $0.02 per minute is very reasonable
- **Large files need splitting** - 25MB limit per request
- **Response formats vary** - JSON, text, SRT, VTT for different uses
- **Perfect for accessibility** - makes audio content searchable and accessible

## = Resources

- [OpenAI Whisper API Documentation](https://platform.openai.com/docs/guides/speech-to-text)
- [Whisper Model on GitHub](https://github.com/openai/whisper)
- [Supported Audio Formats](https://platform.openai.com/docs/guides/speech-to-text/supported-file-types)
- [Prompting for Accuracy](https://platform.openai.com/docs/guides/speech-to-text/prompting)
- [Pricing Details](https://openai.com/pricing/)

##  Completion Checklist

- [ ] Set up Whisper API access
- [ ] Created or found an audio file for testing
- [ ] Performed basic transcription
- [ ] Used context prompts to improve accuracy
- [ ] Tested translation feature (audio to English)
- [ ] Experimented with different response formats (JSON, SRT, VTT)
- [ ] Processed multiple audio files in batch
- [ ] Understood file size limitations and chunking
- [ ] Built a simple meeting transcription system
- [ ] Know practical applications (captions, accessibility, search)

## = Up Next

Tomorrow: [Day 24 - Fine-tuning Basics](./day24.md) - Learn how to customize models to your specific needs!

---

**Progress**: Day 23 of 30
