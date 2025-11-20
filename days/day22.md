# Day 22: Text-to-Speech with OpenAI

## =Ì Objective
Master OpenAI's text-to-speech API to generate natural-sounding audio from text, and understand applications in accessibility, automation, and voice-based interfaces.

## <¯ Today's Exercise (10-15 minutes)

### Prerequisites
- Python 3.8+
- `pip install openai python-dotenv`
- OpenAI API key with TTS access

### Part 1: Understanding OpenAI TTS (2 min)

**What is Text-to-Speech?**

OpenAI's TTS API converts text to human-quality speech with:
- Multiple voice options (6 voices available)
- Adjustable speaking speed
- Multiple output formats (MP3, opus, aac, flac)
- Low latency for real-time applications
- Natural intonation and emotional tone

**Available Voices**:
1. **Alloy**: Neutral, versatile
2. **Echo**: Warm, friendly
3. **Fable**: Distinctive, energetic
4. **Onyx**: Deep, professional
5. **Nova**: Clear, enthusiastic
6. **Shimmer**: Bright, expressive

**Use Cases**:
- Accessibility tools for visually impaired users
- Audiobook generation
- Voice notifications in apps
- Automated customer service
- Podcast creation
- Educational content

### Part 2: Basic TTS Implementation (5 min)

**Create `tts_demo.py`**:

```python
import os
from openai import OpenAI
from pathlib import Path
from dotenv import load_dotenv

load_dotenv()

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

print("="*60)
print("TEXT-TO-SPEECH EXAMPLES")
print("="*60)

# Example 1: Simple text-to-speech
text = "Hello! I'm an AI assistant. I can now speak to you!"
response = client.audio.speech.create(
    model="tts-1",
    voice="alloy",
    input=text
)

with open("output.mp3", "wb") as f:
    f.write(response.content)
print("Saved audio to output.mp3")

# Example 2: Different voices
voices = ["alloy", "echo", "fable", "onyx", "nova", "shimmer"]
for voice in voices:
    response = client.audio.speech.create(
        model="tts-1",
        voice=voice,
        input="Welcome to OpenAI Text-to-Speech"
    )
    with open(f"voice_{voice}.mp3", "wb") as f:
        f.write(response.content)

# Example 3: Adjusting speed
speeds = [0.5, 0.75, 1.0, 1.25]
for speed in speeds:
    response = client.audio.speech.create(
        model="tts-1",
        voice="nova",
        speed=speed,
        input="This is a test of different speeds"
    )
    with open(f"speed_{speed}.mp3", "wb") as f:
        f.write(response.content)

print("TTS generation complete!")
```

**Run it**:
```bash
python tts_demo.py
```

### Part 3: Integration Concept (3 min)

**Streaming Response** (for real-time applications):

```python
response = client.audio.speech.create(
    model="tts-1",
    voice="alloy",
    input="Stream this immediately",
    response_format="opus"
)
```

**Cost Considerations**:
- Pricing: ~$15 per 1 million characters
- Standard model: `tts-1` (faster, lower latency)
- High quality model: `tts-1-hd` (better quality, slower)

## =¡ Key Takeaways

- **Text-to-Speech is powerful for accessibility** and reaching diverse audiences
- **Voice selection matters** - each voice has different emotional tones
- **Speech speed is adjustable** - tailor to your use case
- **Text formatting affects output** - use proper punctuation and clarity
- **Chunking needed for long content** - API has character limits
- **Cost-effective for scale** - cheaper than hiring voice actors
- **Low latency model available** - `tts-1` for real-time applications
- **Multiple output formats** - choose based on your platform needs

## = Resources

- [OpenAI Text-to-Speech API Docs](https://platform.openai.com/docs/guides/text-to-speech)
- [TTS Models Comparison](https://platform.openai.com/docs/guides/text-to-speech/quickstart)
- [Audio File Format Guide](https://platform.openai.com/docs/guides/text-to-speech/audio-quality)
- [Pricing Information](https://openai.com/pricing/gpt-4-audio/)
- [OpenAI Python SDK](https://github.com/openai/openai-python)

##  Completion Checklist

- [ ] Installed necessary packages and set up API key
- [ ] Generated speech using basic TTS
- [ ] Experimented with all 6 different voices
- [ ] Tested different speech speeds (0.5x to 1.25x)
- [ ] Generated audio for longer text content
- [ ] Created a voice notification system
- [ ] Understood the difference between tts-1 and tts-1-hd
- [ ] Know how to handle API limits and chunking
- [ ] Listened to generated audio samples
- [ ] Can explain practical applications of TTS

## = Up Next

Tomorrow: [Day 23 - Speech-to-Text with Whisper](./day23.md) - Learn the reverse: transcribing audio to text with state-of-the-art accuracy!

---

**Progress**: Day 22 of 30
