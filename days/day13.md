# Day 13: Vision Models - Multimodal AI

## =Ì Objective
Explore how modern AI models can process both text and images together, understanding multimodal capabilities and practical applications like image analysis and visual question answering.

## <¯ Today's Exercise (10-15 minutes)

### Part 1: Understanding Multimodal AI (3 min)

**What is Multimodal AI?**
- Traditional AI: Works with one type of data (text OR images OR audio)
- Multimodal AI: Processes multiple data types together (text + images + audio)

**Real-World Applications**:
1. **Image Analysis**: Describe what's in a photo
2. **Visual Q&A**: Answer questions about images
3. **Document Understanding**: Read text from images (OCR)
4. **Product Recommendations**: Match text descriptions to images
5. **Content Moderation**: Identify harmful content in images

**Current Models**:
- **GPT-4 Vision**: OpenAI's multimodal model
- **Claude 3**: Anthropic's vision capabilities
- **Gemini Pro Vision**: Google's multimodal model
- **LLaVA**: Open-source vision model

### Part 2: Hands-On with Vision APIs (8 min)

**Using ChatGPT's Vision Feature**:

1. Open [ChatGPT Web](https://chat.openai.com)
2. Click the image icon or drag an image into the chat
3. Ask questions about the image:

**Try These Questions**:
```
Sample Image: Any screenshot, photo, or diagram

Questions to ask:
- "Describe everything you see in this image"
- "What text appears in this image?"
- "Analyze the layout and structure"
- "What's the main purpose of this image?"
- "Extract any data or numbers you see"
```

**Using Claude with Images**:

1. Go to [Claude.ai](https://claude.ai)
2. Upload an image
3. Try analysis tasks:

```
"I'm uploading a screenshot. Can you:
1. Identify all UI elements
2. Describe the layout
3. Suggest improvements to the design
4. Extract any visible text"
```

**Observe**:
- How detailed are the descriptions?
- Does it understand charts and diagrams?
- Can it read handwritten text?
- How does it handle complex images?

### Part 3: Hands-On Code Example (4 min)

**Using Python with OpenAI Vision** (if you have an API key):

```python
import base64
import requests
import os
from dotenv import load_dotenv

load_dotenv()

def analyze_image(image_path):
    """Analyze an image using GPT-4 Vision"""

    # Encode image to base64
    with open(image_path, "rb") as image_file:
        base64_image = base64.b64encode(image_file.read()).decode('utf-8')

    # Determine image type
    image_type = "image/jpeg" if image_path.endswith('.jpg') else "image/png"

    # Make API call
    headers = {
        "Content-Type": "application/json",
        "Authorization": f"Bearer {os.getenv('OPENAI_API_KEY')}"
    }

    payload = {
        "model": "gpt-4-vision-preview",
        "messages": [
            {
                "role": "user",
                "content": [
                    {
                        "type": "image_url",
                        "image_url": {
                            "url": f"data:{image_type};base64,{base64_image}"
                        }
                    },
                    {
                        "type": "text",
                        "text": "Analyze this image. Describe what you see, identify any text, and summarize the main content."
                    }
                ]
            }
        ],
        "max_tokens": 1024
    }

    response = requests.post(
        "https://api.openai.com/v1/messages",
        headers=headers,
        json=payload
    )

    print("Analysis:")
    print(response.json()['content'][0]['text'])

# Usage
# analyze_image("path/to/image.jpg")
```

**Using Claude with Python**:

```python
import anthropic
import base64

def analyze_with_claude(image_path):
    """Analyze an image using Claude Vision"""

    client = anthropic.Anthropic(api_key="your-api-key")

    # Read and encode image
    with open(image_path, "rb") as img:
        image_data = base64.standard_b64encode(img.read()).decode("utf-8")

    # Determine media type
    media_type = "image/jpeg" if image_path.endswith('.jpg') else "image/png"

    message = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=1024,
        messages=[
            {
                "role": "user",
                "content": [
                    {
                        "type": "image",
                        "source": {
                            "type": "base64",
                            "media_type": media_type,
                            "data": image_data,
                        },
                    },
                    {
                        "type": "text",
                        "text": "What do you see in this image? Describe the content, layout, and any text present."
                    }
                ],
            }
        ],
    )

    print(message.content[0].text)

# Usage
# analyze_with_claude("path/to/image.jpg")
```

## =¡ Key Takeaways

- **Multimodal AI combines text and images**: You can ask questions about visual content
- **Vision models can perform OCR**: Extract text from images and documents
- **Real-world applications are diverse**: From product search to accessibility
- **API integration is straightforward**: Load image as base64, send with text in request
- **Trade-offs exist**: Vision models are slower and more expensive than text-only models
- **Accuracy varies by image type**: Charts, diagrams, and clear photos work best

## = Resources

- [OpenAI Vision Guide](https://platform.openai.com/docs/guides/vision)
- [Claude Vision Capabilities](https://docs.anthropic.com/claude/reference/vision)
- [Google Gemini Vision](https://ai.google.dev/tutorials/multimodal_quickstart)
- [LLaVA Open-Source Vision Model](https://llava-vl.github.io/)
- [Vision Model Benchmarks](https://github.com/clip-by-openai/CLIP)

##  Completion Checklist

- [ ] Understood the difference between traditional and multimodal AI
- [ ] Uploaded and analyzed an image with ChatGPT or Claude
- [ ] Asked follow-up questions about image content
- [ ] Tested image understanding with text extraction
- [ ] Reviewed the Python code examples (if applicable)
- [ ] Observed limitations and edge cases with images

## = Up Next

Tomorrow: [Day 14 - Image to Text with CLIP](./day14.md) - Use CLIP to understand image embeddings and perform advanced image analysis!

---

**Progress**: Day 13 of 30
