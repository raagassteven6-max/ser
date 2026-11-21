---
title: "Trying to Give Qwen a Little More Teeth"
date: 2026-11-21
tags: [AI, Qwen2.5, fine-tuning, language models, LoRA, QLoRA, experiments]
summary: "A small update from the model-training corner: while the pixel-art experiment is still ongoing, I've been spending more time fine-tuning Qwen2.5-3B-Instruct toward a more distinctive, less restrained conversational personality."
---

:::center
# Trying to Give Qwen a Little More Teeth

*A small detour into language models, personality tuning, and the stubborn habit of falling back to default behaviour.*
:::

---

## A Little Update From the Lab

I've been pretty busy lately, both with work and a few personal projects. The pixel-art image generation experiment is still sitting on my desk, and I'm still trying to figure out how to get the model to produce the kind of images I actually want. That one hasn't been abandoned. It's just taking its time. 😂

In the meantime, I've been spending more of my attention on something different: **text generation**.

I've been experimenting with fine-tuning **Qwen2.5-3B-Instruct**, using the base instruct model as the starting point for a conversational personality experiment. The idea is to see how much I can change the way a relatively small language model communicates without trying to turn it into an entirely different model.

And, as usual, it sounds easier than it is.

## The Model: Qwen2.5-3B-Instruct

The starting point is `Qwen/Qwen2.5-3B-Instruct`, a 3-billion-parameter, instruction-tuned language model from the Qwen2.5 family.

In practical terms, this is already a model trained to respond to instructions and participate in conversations. I'm not trying to teach it language from scratch, nor am I building a new foundation model. I'm taking an existing instruct model and experimenting with how its conversational behaviour can be shaped through additional training.

That's an important distinction. This project is a **style and personality fine-tune**, not a knowledge upgrade. The goal isn't to make Qwen suddenly know more about the world, improve its factual knowledge, or become a specialist in a particular subject. I want to change the voice it uses when it responds.

The experiment is currently called **Persona v0.2 — “Teeth”**, and it's built around a simple idea: an assistant can be useful without sounding like a customer-service script.

## What I'm Trying to Teach It

The personality I'm aiming for is more lively, opinionated, spontaneous, and conversational. A little sharper around the edges, too.

I want it to be comfortable with sarcasm, playful teasing, blunt observations, natural profanity when it fits, dark humour, deadpan reactions, occasional tangents, and the ability to disagree instead of automatically validating everything. Basically, less of the overly agreeable assistant voice.

But I don't want it to become an edgy caricature, either.

The target intensity in my model card is around **7.5/10**. Enough bite to make the personality noticeable, but not so much that every response becomes an insult, a rant, or an excuse to swear. The model should still know when to drop the act and respond sincerely. If someone is asking for practical help or talking about something serious, the personality shouldn't get in the way.

That's the part I'm particularly interested in: getting the same conversational character to move naturally between humour and sincerity, rather than having two completely different modes.

### The problems with the earlier attempt

The first experiment gave me a useful starting point, but the output still felt a little too mild. It could be witty, but it often sounded restrained, neatly structured, and predictable. There was also a tendency to follow a familiar pattern: make a joke, explain the point, then wrap everything up with a tidy conclusion.

That isn't necessarily bad writing. It's just not the voice I'm trying to build.

For Dataset2, I'm trying to encourage more variation in response length and rhythm, less canned assistant language, more spontaneous reactions, and humour that actually responds to the conversation instead of appearing as a decorative punchline.

I also want it to have opinions. Not random contrarianism, but the ability to push back when there's a reason to.

## Dataset2: Building the Personality

The current experiment uses a persona conversation dataset I'm calling **Dataset2**.

The model card describes a 100-example preview set for reviewing the intended style before expanding the training data. Its proposed distribution looks like this:

| Conversation category | Target examples | Share |
|---|---:|---:|
| Natural casual conversation | 25 | 25% |
| Banter and roasting | 25 | 25% |
| Sarcasm and blunt wit | 20 | 20% |
| Dark humour and irony | 15 | 15% |
| Rants and conversational riffs | 10 | 10% |
| Sincere and warm tonal switches | 5 | 5% |
| **Total** | **100** | **100%** |

The distribution is intentional. I don't want a dataset made entirely of jokes and insults, because that would teach the model to reach for the same behaviour regardless of context. Ordinary conversation matters just as much. So do examples where the assistant knows when to be serious.

I'm also trying to avoid the trap of generating hundreds of near-identical examples from a handful of prompts. That might make a dataset look larger, but it doesn't necessarily give the model a broader understanding of conversational situations. The eventual dataset needs genuinely different scenarios, multi-turn exchanges, varied response lengths, and examples where the best response is simply a normal answer.

The preview is meant for human review before it becomes the final training set. I still need to see whether the examples have enough variety and whether the personality feels consistent without becoming repetitive.

## The Fine-Tuning Setup

For this iteration, I'm using **supervised fine-tuning (SFT)** with a LoRA/QLoRA approach.

Instead of updating every parameter in the original model, LoRA adds a smaller set of trainable adapter weights. This makes experimentation more manageable, especially when working with a model that I don't need to retrain from the ground up. QLoRA takes that approach further by allowing the base model to be loaded in quantized form during training.

The planned setup for Persona v0.2 is:

| Component | Configuration |
|---|---|
| Base model | `Qwen/Qwen2.5-3B-Instruct` |
| Model size | 3 billion parameters |
| Model type | Instruction-tuned, decoder-only language model |
| Training method | Supervised fine-tuning (SFT) |
| Adaptation | LoRA / QLoRA |
| Base model loading | 4-bit quantization |
| Training tools | Unsloth, Transformers, TRL |
| Initial target | 2 epochs |
| Output | LoRA adapter |
| Dataset | Dataset2 persona conversations |

One decision I'm keeping for this experiment is to start from the original instruct model rather than continue training on top of the previous experiment's adapter. The earlier dataset had some template-heavy and relatively mild examples. Carrying that adapter forward could preserve some of those tendencies and make it harder to tell whether the new dataset is actually improving anything.

Two epochs are the initial comparison setting, not some magic number I've decided is optimal. I'll need to look at the outputs and training behaviour before deciding whether that should change.

And because this is an adapter-based setup, the result isn't a standalone model by itself. The trained LoRA adapter needs to be loaded alongside the compatible Qwen2.5-3B-Instruct base model and tokenizer.

## The Part That's Giving Me Trouble

Here's where things get interesting.

I'm leaning toward a less restrained, more uncensored-feeling conversational behaviour, but I still haven't managed to pin down exactly how to get there. The model keeps falling back to what I can only describe as **generic Qwen behaviour**.

I can ask for a sharper voice. I can put the personality requirements into the prompt. I can prepare examples that are supposed to demonstrate the intended tone. But when I test the model, that familiar assistant style still shows up.

It's a reminder that a personality description and a learned behaviour aren't the same thing. A prompt can ask a model to be blunt, sarcastic, spontaneous, or irreverent. But getting those qualities to appear naturally across different conversations is another problem entirely.

I don't want profanity for the sake of profanity, or a model that mistakes being rude for having a personality. What I'm after is a more convincing shift in its conversational instincts: how it reacts, how it phrases things, when it jokes, when it pushes back, and when it decides not to make a joke at all.

There are several things I still need to watch for:

- **Personality strength:** Does the model actually sound sharper, or does it just add a joke to an otherwise generic answer?
- **Naturalness:** Does profanity or sarcasm fit the exchange, rather than appearing because the dataset tells it to?
- **Variety:** Does it change its rhythm and response structure, or repeat the same punchlines and patterns?
- **Context awareness:** Can it tell when teasing is welcome and when sincerity matters more?
- **General usefulness:** Does the fine-tune preserve the base model's ability to follow instructions and answer practical questions?

A small persona dataset can also overfit. It might teach the model to repeat certain phrases, exaggerate a particular style, or lose some of the instruction-following behaviour that made the base model useful in the first place. So I'm not treating a few entertaining generations as proof that the experiment worked.

## What Comes Next?

For now, I'm continuing with Dataset2 and testing the outputs against the original base model using the same prompts and generation settings. I want a proper side-by-side comparison, not just a handful of cherry-picked responses.

I'll be looking at personality strength, spontaneity, humour, response variety, repetition, and whether it can switch back to a sincere and helpful tone when the conversation calls for it. I'll also need to keep an eye on whether the fine-tuning causes any regression in ordinary instruction following.

The model card is still a work in progress. Training metrics, the exact final dataset composition, adapter details, and evaluation results will be added once those parts are actually completed and checked.

I'm also not calling this a finished release yet. **Persona v0.2 “Teeth” is still experimental.**

Once I'm happy with the results, I'll publish the model and its model card on Hugging Face and link it here. Until then, there's more dataset work and probably a few more rounds of asking the model why it keeps sounding like the default assistant. 😅

:::center
*Still tuning the voice. Still negotiating with the model.*

**STATUS: FINE-TUNING IN PROGRESS.**
:::

---

## Project Notes

- **Base model:** [Qwen2.5-3B-Instruct](https://huggingface.co/Qwen/Qwen2.5-3B-Instruct)
- **Experiment:** Persona v0.2 — “Teeth”
- **Dataset:** Dataset2 (persona conversation dataset; preview and composition subject to revision)
- **Training approach:** SFT with LoRA / QLoRA
- **Tooling:** [Unsloth](https://github.com/unslothai/unsloth), [Hugging Face Transformers](https://github.com/huggingface/transformers), [TRL](https://github.com/huggingface/trl)
- **Release status:** Experimental; Hugging Face adapter link to be added after training and evaluation

*This is a personal fine-tuning experiment and is not an official Qwen or Unsloth release.*
