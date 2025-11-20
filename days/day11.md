# Day 11: Image Prompting Techniques =ø

## =Ì Objective
Master advanced image prompting strategies that work across DALL-E, Midjourney, and Stable Diffusion to create exactly what you envision.

## <¯ Today's Exercise (10-15 minutes)

### Universal Prompting Framework

**The DETAILS Method**:
- **D**escription: What is it?
- **E**nvironment/Setting: Where?
- **T**one/Mood: What feeling?
- **A**rt Style: How should it look?
- **I**llumination: Lighting?
- **L**ens/Perspective: Viewpoint?
- **S**pecificity: Quality markers?

### Part 1: Composition Techniques (5 min)

**1. Camera Angles & Perspectives**

Test these prompts (pick your favorite AI image tool):

```
Subject: "A vintage red bicycle"

A. Eye level, straight on, centered composition
B. Low angle looking up, dramatic perspective
C. Bird's eye view, directly from above, flat lay
D. Dutch angle, tilted 45 degrees, dynamic composition
E. Extreme close-up, macro photography, shallow depth of field
```

**Observe**: How does perspective completely change the image?

**2. Framing & Composition**

```
Subject: "A cozy coffee shop"

A. Wide establishing shot, full interior visible
B. Medium shot, focus on barista counter
C. Close-up, steaming coffee cup with latte art
D. Over-the-shoulder view of customer
E. Through window looking in, reflections visible
```

### Part 2: Lighting Mastery (5 min)

**Lighting Types to Try**:

```
Base prompt: "Portrait of a thoughtful artist in their studio"

1. Golden hour: soft warm light from window, sunset glow
2. Hard dramatic: single spotlight, deep shadows, film noir style
3. Soft diffused: overcast natural light, even, no harsh shadows
4. Backlighting: subject silhouetted, rim light, ethereal
5. Colored gel: neon pink and blue lighting, cyberpunk atmosphere
```

**Pro Lighting Keywords**:
- Natural: "window light", "diffused sunlight", "overcast"
- Dramatic: "Rembrandt lighting", "chiaroscuro", "high contrast"
- Studio: "three-point lighting", "softbox", "rim light"
- Creative: "volumetric rays", "god rays", "atmospheric lighting"

### Part 3: Quality & Style Modifiers (5 min)

**The Quality Stack**:

Test adding these progressively to any prompt:

```
Base: "Ancient library"

Level 1: Ancient library, detailed

Level 2: Ancient library, detailed, 8k, high quality

Level 3: Ancient library, detailed, 8k, high quality, professional
photography, award-winning

Level 4: Ancient library, intricate details, 8k uhd, high quality,
professional architectural photography, award-winning, National
Geographic style, perfect composition
```

**Quality Modifiers by Category**:

**Resolution**:
- 4k, 8k, 16k uhd
- high resolution, extremely detailed
- intricate details, fine details

**Photography**:
- shot on [camera]: Canon 5D, Hasselblad, Leica
- lens: 85mm f/1.4, wide angle, telephoto
- film: Kodak Portra, Fujifilm, Cinestill

**Art Quality**:
- masterpiece, museum quality
- trending on ArtStation
- award-winning, professional
- by [famous artist]

**Rendering** (for 3D/Digital):
- octane render, unreal engine 5
- ray tracing, global illumination
- volumetric lighting, subsurface scattering

## =¡ Key Takeaways

**Prompt Building Order**:
```
1. Subject (what)
2. Action/Pose (doing what)
3. Setting/Environment (where)
4. Lighting (how lit)
5. Style/Medium (what kind of art)
6. Camera/Perspective (viewpoint)
7. Quality modifiers (enhancement)
8. Mood/Atmosphere (feeling)
```

**Example Application**:
```
[1-Subject] Elderly fisherman
[2-Action] mending nets
[3-Setting] on weathered wooden dock, small coastal village background
[4-Lighting] warm morning sunlight, golden hour
[5-Style] realistic oil painting
[6-Camera] medium shot, eye level, rule of thirds composition
[7-Quality] highly detailed, masterful brushwork, rich colors
[8-Mood] peaceful, nostalgic atmosphere

Final Prompt: "Elderly fisherman mending nets on weathered wooden
dock, small coastal village in background, warm morning sunlight,
golden hour, realistic oil painting, medium shot, eye level, rule
of thirds composition, highly detailed, masterful brushwork, rich
colors, peaceful nostalgic atmosphere"
```

**Cross-Tool Compatibility**:

| Technique | DALL-E 3 | Midjourney | Stable Diffusion |
|-----------|----------|------------|------------------|
| Detailed descriptions |  Excellent |  Excellent |  Excellent |
| Artist references |   Limited |  Excellent |  Excellent |
| Camera specs |  Good |  Good |  Excellent |
| Negative prompts | L No |   --no flag |  Native |
| Quality modifiers |  Good |   Can over-fit |  Important |

## =' Style Reference Library

**Photography Styles**:
```
- Fashion: "Vogue photoshoot, editorial, high fashion"
- Documentary: "photojournalism, candid, National Geographic"
- Fine Art: "Annie Leibovitz style, conceptual, artistic"
- Product: "commercial photography, studio lighting, e-commerce"
```

**Painting Styles**:
```
- Classical: "oil on canvas, Renaissance, chiaroscuro"
- Impressionist: "loose brushstrokes, Monet style, plein air"
- Modern: "abstract expressionism, bold colors, gestural"
- Digital: "digital painting, concept art, matte painting"
```

**Film & Cinema**:
```
- "cinematic composition, anamorphic lens, film grain"
- "Wes Anderson style, symmetrical, pastel colors"
- "Christopher Nolan, IMAX, epic scale"
- "Studio Ghibli, anime background art, detailed"
```

## = Resources

- [PromptBase](https://promptbase.com/) - Marketplace for prompts
- [Lexica](https://lexica.art/) - Visual prompt search
- [OpenArt](https://openart.ai/) - Prompt engineering playground
- [DALLERY](https://dallery.gallery/) - DALL-E prompt guide

##  Completion Checklist

- [ ] Tested at least 3 different camera angles for the same subject
- [ ] Experimented with 3+ lighting scenarios
- [ ] Built a prompt using the 8-step framework
- [ ] Understand how quality modifiers affect output
- [ ] Created a personal reference of favorite style keywords
- [ ] Generated images showing before/after adding details

## <¯ Challenge

**Create a "prompt evolution" series**:

Start with: "A dragon"

Iteration 1: Add environment
Iteration 2: Add lighting and time of day
Iteration 3: Add art style
Iteration 4: Add camera perspective
Iteration 5: Add quality and mood modifiers

Generate an image at each iteration and see the dramatic improvement!

## = Up Next

Tomorrow: [Day 12 - Hugging Face Diffusers](./day12.md) - Code your own image generation with Python!

---

**Progress**: Day 11 of 30 
