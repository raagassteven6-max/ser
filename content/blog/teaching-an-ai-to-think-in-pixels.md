---
title: "Teaching an AI to Think in Pixels"
date: 2026-11-16
tags: [AI, Stable Diffusion, fine-tuning, pixel art, machine learning, experiments]
summary: "A small update from the pixel-art laboratory: trying to teach Stable Diffusion 1.5 that 16-bit and 32-bit art is about working within limits, not just looking pixelated."
---

:::center

# Teaching an AI to Think in Pixels

*An experiment in making a very capable model learn how to do less.*

:::

---

I've been a little occupied with work and a few other projects over the past several days, so I haven't had as much time to work on my personal stuff as I'd like. That said, I've been spending quite a bit of time on one particular experiment: fine-tuning an image-generation model.

The model I'm currently working with is **Stable Diffusion 1.5**.

For some context, SD 1.5 has approximately **1.07 billion parameters** spread across its main components:

- **UNet:** approximately 860 million parameters
- **CLIP text encoder:** approximately 123 million parameters
- **VAE:** approximately 84 million parameters

That's a lot of parameters to convince to draw tiny squares properly. 😭

## The Pixel Problem

The idea behind this experiment is pretty straightforward. I want to see if I can fine-tune the model to generate convincing **16-bit to 32-bit-style pixel art**, including more detailed illustrations and dynamic compositions.

I've always found this particular style interesting, especially how much detail artists can squeeze out of such limited resolutions and color palettes. So naturally, I wanted to see whether I could get an image-generation model to do something similar.

There's just one problem.

When I ask most modern image-generation models to produce pixel art, I sometimes get something that looks like a regular digital illustration with a pixelation filter applied to it. It might have visible squares and jagged edges, but that doesn't necessarily make it proper pixel art.

These models have been trained to produce highly detailed images, with smooth gradients, realistic lighting, complex textures, and all sorts of little visual details. That's great for general image generation, but it can become a bit of a problem when you're trying to teach one to work within the limitations of pixel art.

Pixel art requires a different way of approaching an image. You have to think about how shapes read at smaller resolutions, where individual pixels should go, how colors interact, and how much detail you can actually fit into a limited space. Even something as simple as an outline or a shadow needs to be considered carefully.

You can't just throw more detail at the image and expect it to look better.

**What I'm really trying to achieve is an image that looks like it was deliberately created within pixel-art constraints, rather than an ordinary image that happens to look pixelated.**

And that's proving to be a little more difficult than I initially expected.

## The Dataset Rabbit Hole

A good portion of my time has gone into collecting, sorting, inspecting, and curating datasets. Quite a bit of it, actually.

I quickly realized that this isn't something I can approach by simply downloading every image tagged *pixel art* and throwing everything into a training folder. There's a lot more to consider, particularly when the goal is to teach the model a specific visual style.

Resolution, palette consistency, image quality, composition, and even the captions accompanying the images can affect what the model eventually learns.

I've been looking at several types of material, including:

- **Native-resolution sprites:** small character sprites, objects, items, creatures, and environmental assets.
- **16×16 and 24×24 assets:** useful for studying compact shapes, silhouettes, and very limited color arrangements.
- **32×32 and 64×64 sprites:** useful for more detailed subjects while still retaining a deliberate pixel grid.
- **Pixel-art illustrations and scenes:** landscapes, interiors, architecture, backgrounds, and larger compositions.
- **Sprite sheets and animation frames:** useful visual references for consistent shapes, poses, and character details.
- **Palette-focused examples:** artwork that demonstrates how a limited set of colors can still communicate lighting, depth, and material.

I've also come across a few publicly available datasets that are worth mentioning, especially for anyone interested in experimenting with pixel-art generation:

1. [Pixel Art Atlas — 1.4M Synthetic Sprites](https://huggingface.co/datasets/Angais/pixel-art-atlas-1.4m) — a large collection of native 16×16 synthetic sprites with text captions. Its dataset card lists a CC0 license.
2. [PixelGPT 24×24 Dataset](https://huggingface.co/datasets/unstonio/pixelgpt-24x24-20k) — a collection of 24×24 pixel-art sprites with captions, palette information, and category metadata. The listed license is CC BY 4.0.
3. [MOSS Pixel-Art Sprites 64](https://huggingface.co/datasets/ProbablyHumanAB/moss-pixelart-64) — a smaller collection of 64×64 synthetic sprites paired with generation prompts, listed under Apache 2.0.

These are some of the resources I've found while exploring the dataset landscape. That doesn't necessarily mean all of them are included in my current training set, though. I'm still going through the material and deciding what actually makes sense to use.

I'd rather spend time putting together a smaller, more consistent collection than feed the model a massive assortment of images that don't necessarily work well together. More data isn't automatically better, particularly when the training objective is this specific.

Licensing is another thing I'm keeping an eye on. Just because an image is publicly available online doesn't mean it's automatically free to use for training. I'd rather check the licensing and provenance of the material than blindly collect everything I come across.

## Trying to Reprogram Its Artistic Instincts

Now, onto the actual fine-tuning.

Let's just say the results so far have been... interesting.

I've already gone through **three versions**, and, well, they haven't been particularly impressive. The outputs are still frustratingly similar, despite the different iterations I've tried.

It's almost like trying to take a very modern digital artist, send them back in time, and tell them they can only work with a tiny canvas, a handful of colors, and a limited number of pixels.

Except this artist has over a billion parameters and keeps finding ways to make things unnecessarily complicated.

I'm basically trying to brainwash a model into thinking its limitations are features rather than problems. 🤣

Jokes aside, one of the things I'm trying to figure out is how to get it to understand the differences between various pixel-art resolutions and styles. A small 16×16 sprite has very different requirements from a detailed 32-bit-style illustration. They might fall under the same general category, but the way they're constructed and the amount of detail they can accommodate are quite different.

I don't want the model to simply memorize what a particular type of pixel art looks like. Ideally, it should be able to produce different subjects and compositions while still respecting the visual constraints of the style.

There's still plenty of experimenting to do. I'm looking at dataset balance, captions, image consistency, training settings, and, of course, the outputs themselves. I also need to be a little more systematic about comparing each version rather than just judging them individually.

So far, I wouldn't consider any of the three versions successful. There's definitely room for improvement, and I'm still trying to figure out what needs to change.

## What's Next?

For now, I'm putting some time into organizing the datasets, reviewing the generated samples, and comparing the different model versions. I want to get the results to a reasonable standard before making anything publicly available.

Eventually, I'd like to list the fine-tuned models here, together with some sample generations and perhaps a bit of documentation covering the training process. It would be nice to have everything in one place, especially if someone else wants to experiment with the models.

But I'd rather take my time with that than publish something half-baked and pretend it's ready.

For the time being, the models are staying in the workshop.

There's still a lot to figure out, and knowing how these projects usually go, I'll probably find another way to make the whole thing considerably more complicated than it needs to be.

Anyway, that's what I've been working on lately.

My tiny pixel laboratory remains operational. More or less.

:::center

*One billion parameters. A handful of colors. And an unreasonable amount of stubbornness.*

**See ya!** 👋

`STATUS: TRAINING... PLEASE WAIT.`

:::

---

### A few references

- [Stable Diffusion 1.5 — Model architecture and parameter breakdown](https://ostris.com/docs/ai-toolkit/models/sd15)
- [Pixel Art Atlas — Hugging Face](https://huggingface.co/datasets/Angais/pixel-art-atlas-1.4m)
- [PixelGPT 24×24 — Hugging Face](https://huggingface.co/datasets/unstonio/pixelgpt-24x24-20k)
- [MOSS Pixel-Art Sprites 64 — Hugging Face](https://huggingface.co/datasets/ProbablyHumanAB/moss-pixelart-64)
