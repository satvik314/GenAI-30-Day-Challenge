# Day 12: Hugging Face Diffusers - Code Your Own Images >

## =Ì Objective
Use the Hugging Face Diffusers library to programmatically generate images with Stable Diffusion using Python.

## <¯ Today's Exercise (10-15 minutes)

### What is Hugging Face Diffusers?

**Diffusers** is a Python library that makes it easy to:
- Generate images programmatically
- Use various Stable Diffusion models
- Fine-tune and customize generation
- Build image generation into applications

### Part 1: Setup & First Image (5 min)

**Installation**:
```bash
pip install diffusers transformers accelerate torch
```

**Basic Image Generation** (`generate_image.py`):

```python
from diffusers import StableDiffusionPipeline
import torch

# Load the model (first time will download ~4GB)
model_id = "runwayml/stable-diffusion-v1-5"
pipe = StableDiffusionPipeline.from_pretrained(
    model_id,
    torch_dtype=torch.float16  # Use FP16 for speed
)

# Move to GPU if available
pipe = pipe.to("cuda" if torch.cuda.is_available() else "cpu")

# Generate image
prompt = "a serene Japanese garden with cherry blossoms, koi pond, traditional architecture, peaceful atmosphere, high quality"

image = pipe(
    prompt=prompt,
    num_inference_steps=30,
    guidance_scale=7.5
).images[0]

# Save the image
image.save("japanese_garden.png")
print("Image saved!")
```

**Run it**:
```bash
python generate_image.py
```

### Part 2: Advanced Parameters (5 min)

**Controlling Generation** (`advanced_generation.py`):

```python
from diffusers import StableDiffusionPipeline
import torch

pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16
).to("cuda" if torch.cuda.is_available() else "cpu")

# Advanced configuration
prompt = "futuristic cityscape at night, neon lights, flying cars, cyberpunk, detailed"
negative_prompt = "blurry, low quality, distorted, ugly"

image = pipe(
    prompt=prompt,
    negative_prompt=negative_prompt,
    num_inference_steps=50,  # More steps = better quality (slower)
    guidance_scale=7.5,       # How closely to follow prompt (1-20)
    width=512,
    height=512,
    num_images_per_prompt=1,
    seed=42  # For reproducibility
).images[0]

image.save("cyberpunk_city.png")
```

**Parameter Guide**:
- `num_inference_steps`: 20-50 (higher = better quality, slower)
- `guidance_scale`: 7-12 (higher = follows prompt more strictly)
- `seed`: Same seed = same image (useful for iterations)

### Part 3: Batch Generation (5 min)

**Generate Multiple Variations**:

```python
from diffusers import StableDiffusionPipeline
import torch

pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16
).to("cuda" if torch.cuda.is_available() else "cpu")

# Multiple prompts
prompts = [
    "a cozy reading nook with books and warm lighting",
    "a futuristic workspace with holographic displays",
    "a magical forest with glowing mushrooms at night"
]

for i, prompt in enumerate(prompts):
    image = pipe(
        prompt=prompt,
        num_inference_steps=30,
        guidance_scale=7.5
    ).images[0]

    image.save(f"image_{i+1}.png")
    print(f"Generated image {i+1}/3")

print("All images generated!")
```

## =¡ Key Takeaways

**Pipeline Types**:
```python
# Text to Image
from diffusers import StableDiffusionPipeline

# Image to Image (modify existing images)
from diffusers import StableDiffusionImg2ImgPipeline

# Inpainting (edit parts of images)
from diffusers import StableDiffusionInpaintPipeline

# Upscaling
from diffusers import StableDiffusionUpscalePipeline
```

**Model Selection**:
```python
# SD 1.5 - Fast, good quality
"runwayml/stable-diffusion-v1-5"

# SD 2.1 - Better quality
"stabilityai/stable-diffusion-2-1"

# SDXL - Highest quality (requires more VRAM)
"stabilityai/stable-diffusion-xl-base-1.0"
```

**Performance Tips**:
- Use `torch.float16` for faster generation
- Lower resolution (512x512) is faster than (1024x1024)
- Fewer steps = faster (but lower quality)
- Batch processing is more efficient

## =' Practical Application

**Image Generation Service**:

```python
from diffusers import StableDiffusionPipeline
import torch
from datetime import datetime

class ImageGenerator:
    def __init__(self):
        self.pipe = StableDiffusionPipeline.from_pretrained(
            "runwayml/stable-diffusion-v1-5",
            torch_dtype=torch.float16
        ).to("cuda" if torch.cuda.is_available() else "cpu")

    def generate(self, prompt, negative_prompt="", steps=30, scale=7.5):
        """Generate image with given parameters"""
        image = self.pipe(
            prompt=prompt,
            negative_prompt=negative_prompt,
            num_inference_steps=steps,
            guidance_scale=scale
        ).images[0]

        # Save with timestamp
        filename = f"generated_{datetime.now().strftime('%Y%m%d_%H%M%S')}.png"
        image.save(filename)
        return filename

# Usage
generator = ImageGenerator()
result = generator.generate(
    prompt="beautiful sunset over mountains",
    negative_prompt="blurry, low quality"
)
print(f"Image saved as: {result}")
```

## = Resources

- [Hugging Face Diffusers Docs](https://huggingface.co/docs/diffusers)
- [Model Hub](https://huggingface.co/models?pipeline_tag=text-to-image)
- [Diffusers Examples](https://github.com/huggingface/diffusers/tree/main/examples)
- [Stable Diffusion Guide](https://huggingface.co/blog/stable_diffusion)

##  Completion Checklist

- [ ] Installed diffusers library successfully
- [ ] Generated your first image with code
- [ ] Experimented with different guidance_scale values (5, 7.5, 12)
- [ ] Used negative prompts to improve quality
- [ ] Generated a batch of images with different prompts
- [ ] Understand the key parameters and their effects
- [ ] (Optional) Tried different models from Hugging Face

## <¯ Challenge

Create a script that:
1. Accepts user input for a prompt
2. Generates 4 variations with different seeds
3. Saves all images with descriptive filenames
4. Prints generation time for each image

Bonus: Add a progress bar using `tqdm`!

## = Up Next

Tomorrow: [Day 13 - Vision Models: Multimodal AI](./day13.md) - Explore AI that understands both images and text!

---

**Progress**: Day 12 of 30 
