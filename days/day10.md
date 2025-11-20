# Day 10: Stable Diffusion - Open Source Image Generation =

## =Ì Objective
Explore Stable Diffusion, the open-source image generation model, and understand the benefits of local, free AI image creation.

## <¯ Today's Exercise (10-15 minutes)

### What is Stable Diffusion?

**Stable Diffusion** is an open-source text-to-image model that:
- Runs locally on your computer (free!)
- Offers complete creative freedom
- Has a massive community creating custom models
- Provides full control over the generation process

**Key Advantage**: No usage limits, full privacy, completely free

### Part 1: Ways to Use Stable Diffusion (5 min)

**Option 1: Online (Easiest - No installation)**

Try these free platforms:

1. **DreamStudio** (Official): https://beta.dreamstudio.ai
   - Free credits to start
   - Simple interface
   - Good for beginners

2. **Hugging Face Spaces**: https://huggingface.co/spaces
   - Search for "Stable Diffusion"
   - Free, no signup needed
   - May have queues during busy times

3. **Clipdrop**: https://clipdrop.co/stable-diffusion
   - Clean interface
   - Quick results

**Option 2: Local Installation** (Advanced)
- Requires: Good GPU (NVIDIA with 6GB+ VRAM recommended)
- Tools: Automatic1111 WebUI, ComfyUI
- We'll skip detailed setup today (time intensive)

### Part 2: Understanding SD Models (5 min)

**Base Models**:

1. **SD 1.5**: Original, widely supported
2. **SD 2.1**: Improved quality
3. **SDXL**: Latest, highest quality (slower)

**Custom Models** (Fine-tuned versions):
- **Realistic Vision**: Photorealistic images
- **DreamShaper**: Balanced, artistic
- **Anime models**: Anything V3, Counterfeit
- **Artistic**: Deliberate, Protogen

**Where to find**: https://civitai.com/ (community models)

### Part 3: Hands-On Exercise (5 min)

**Use DreamStudio or Hugging Face Space** and try these prompts:

**1. Basic Prompt**:
```
Positive: a serene mountain lake at sunrise, mist on water,
pine trees, realistic, detailed, 8k

Negative: blurry, low quality, distorted, ugly, watermark
```

**2. Portrait**:
```
Positive: portrait of a wise elderly wizard, long white beard,
mystical robes, magical staff, detailed, fantasy art, trending
on artstation

Negative: young, modern, blurry, deformed, multiple faces
```

**3. Product/Object**:
```
Positive: steampunk pocket watch, intricate gears visible,
brass and copper, ornate engravings, studio lighting, product
photography, highly detailed

Negative: simple, modern, plastic, low quality, blur
```

**Key Insight**: Negative prompts are crucial in Stable Diffusion!

## =¡ Key Takeaways

**Stable Diffusion Concepts**:

**Positive Prompt**: What you want
```
beautiful garden, flowers, sunlight, vibrant colors, detailed
```

**Negative Prompt**: What to avoid
```
ugly, blurry, low quality, distorted, watermark, text, signature
```

**Important Parameters**:

1. **CFG Scale** (Guidance Scale): 7-12
   - How closely to follow your prompt
   - Higher = more literal (can over-fit)
   - Lower = more creative freedom

2. **Steps**: 20-50
   - Denoising iterations
   - More = potentially better (diminishing returns)
   - Sweet spot: 20-30 for speed

3. **Sampler**: Euler a, DPM++ 2M
   - Algorithm for generating images
   - Euler a: Fast, good quality
   - DPM++ 2M: Higher quality, slower

4. **Seed**: Random number
   - Same seed + prompt = same image
   - Useful for iterations

**Comparison Table**:

| Feature | Stable Diffusion | DALL-E 3 | Midjourney |
|---------|------------------|----------|------------|
| **Cost** | Free (local) | $$ | $$$ |
| **Speed** | Depends on GPU | Fast | Fast |
| **Control** | Maximum | Limited | Medium |
| **Quality** | Excellent | Excellent | Exceptional |
| **Ease of Use** | Complex | Easy | Medium |
| **Customization** | Infinite | None | Limited |

**When to Use Stable Diffusion**:
-  Need unlimited generations
-  Want full creative control
-  Privacy concerns (local generation)
-  Custom model fine-tuning
-  Specific artistic styles (via custom models)

**When to Use Alternatives**:
- L Need quick results without setup
- L Don't have capable GPU
- L Want simplicity over control

## =' Common Negative Prompt Template

Save this for consistent quality:

```
Negative: blurry, low quality, distorted, deformed, ugly,
mutated, disfigured, out of frame, watermark, signature,
text, bad anatomy, bad proportions, extra limbs, cloned face,
duplicate, grain, low-res, worst quality
```

## <¨ Style Keywords for SD

**Realism**:
```
photorealistic, 8k uhd, dslr, soft lighting, high quality,
film grain, Fujifilm XT3
```

**Artistic**:
```
oil painting, canvas, brushstrokes, [artist name] style,
impressionist, vibrant colors, artistic interpretation
```

**3D Render**:
```
3d render, octane render, unreal engine, raytracing,
volumetric lighting, highly detailed, 4k, CGI
```

**Anime**:
```
anime style, manga, cel shaded, vibrant, studio quality,
official art, light novel illustration
```

## = Resources

- [Stability AI](https://stability.ai/) - Official site
- [Civitai](https://civitai.com/) - Custom models community
- [Lexica.art](https://lexica.art/) - SD prompt search engine
- [PromptHero](https://prompthero.com/) - Prompt examples
- [Automatic1111 WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) - Local installation

##  Completion Checklist

- [ ] Tried Stable Diffusion via online platform (DreamStudio or HF)
- [ ] Generated at least 3 images with different prompts
- [ ] Understand the importance of negative prompts
- [ ] Know the key parameters: CFG Scale, Steps, Sampler
- [ ] Explored Civitai or Lexica for inspiration
- [ ] Understand when to use SD vs commercial alternatives
- [ ] Saved effective positive and negative prompt templates

## <¯ Challenge

**Create a character** with consistent features across 3 different scenarios:

1. Portrait (close-up)
2. Full body (action pose)
3. Environmental (in context)

Use the SAME seed and similar prompts to maintain consistency!

**Tip**: Note the seed from image 1, then reuse it for images 2 and 3 with modified prompts.

## = Up Next

Tomorrow: [Day 11 - Image Prompting Techniques](./day11.md) - Master advanced techniques that work across all image AI tools!

---

**Progress**: Day 10 of 30 

**<‰ Week 1 Complete!** You've learned the foundations of GenAI and explored multiple image generation tools. Great work!
