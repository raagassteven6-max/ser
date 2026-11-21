---
title: "Trying to Give Qwen a Little More Teeth"
date: 2026-11-21
tags: [AI, Qwen2.5, fine-tuning, language models, LoRA, QLoRA, experiments]
summary: "A quick update from the model-training corner: the pixel-art experiment is still underway, but lately I've been putting more time into fine-tuning Qwen2.5-3B-Instruct to give it a more distinctive, less restrained conversational personality."
---

:::center
# Trying to Give Qwen a Little More Teeth

*A small detour into language models, personality tuning, and the stubborn habit of slipping back into default behaviour.*
:::

---

## A Little Update From the Lab

I've had a lot on my plate lately, between work and a few personal projects. The pixel-art image-generation experiment is still there, too. I'm still trying to get the model to produce the kind of images I actually have in mind. I haven't dropped that project; it's just taking longer than I'd hoped. 😂

Lately, though, most of my attention has shifted to something else: **text generation**.

I've been experimenting with fine-tuning **Qwen2.5-3B-Instruct**, starting with the original instruct model and seeing how far I can push its conversational personality. I'm curious how much I can change the way a relatively small language model communicates without turning it into something entirely different.

As it turns out, that's easier said than done.

## The Model: Qwen2.5-3B-Instruct

I'm starting with `Qwen/Qwen2.5-3B-Instruct`, a 3-billion-parameter, instruction-tuned model from the Qwen2.5 family.

The model already knows how to follow instructions and take part in conversations. I'm not teaching it language from scratch or building a new foundation model. I'm taking an existing instruct model and testing how additional training can shape the way it talks.

That distinction matters. This is a **style and personality fine-tune**, not an attempt to upgrade the model's knowledge. I'm not expecting Qwen to suddenly know more about the world, become more factually accurate, or turn into a specialist in a particular subject. What I want to change is its voice.

The experiment is currently called **Persona v0.2 — “Teeth”**. The idea behind it is pretty straightforward: an assistant can be helpful without sounding like it's reading from a customer-service script.

## What I'm Trying to Teach It

I'm aiming for a personality that's more lively, opinionated, spontaneous, and conversational. It should have a bit more edge, too.

I'd like it to handle sarcasm, playful teasing, blunt observations, profanity when it fits, dark humour, deadpan reactions, and the occasional tangent. It should also be able to disagree instead of automatically agreeing with everything. In short, I want less of that overly agreeable assistant voice.

There's a line, though. I don't want it turning into an edgy caricature.

The target intensity in my model card is around **7.5/10**. The personality should be noticeable, with enough bite to keep things interesting, but it shouldn't turn every reply into an insult, a rant, or an excuse to swear. It still needs to recognise when to drop the act and respond sincerely. If someone needs practical help or is dealing with something serious, the personality shouldn't get in the way.

That's probably the part I'm most interested in: making one consistent conversational character move naturally between humour and sincerity, without making it feel like the model has two completely separate personalities.

### The problems with the earlier attempt

The first experiment gave me something to build on, but the results were still a little too tame. The model could be witty, yet its replies often felt restrained, neatly arranged, and predictable. It also leaned on the same formula: make a joke, explain the point, and finish with a tidy conclusion.

There's nothing inherently wrong with that style. It just isn't what I'm aiming for here.

With Dataset2, I want to give it more room to vary its response length and rhythm. I'm trying to move away from canned assistant phrasing and encourage reactions that feel spontaneous. The humour should come from what's actually happening in the conversation, not feel like a punchline pasted on at the end.

I want it to have opinions, too. Not to disagree for the sake of disagreeing, but to push back when there's a good reason.

## Dataset2: Building the Personality

For this round, I'm working with a persona conversation dataset I'm calling **Dataset2**.

The model card lays out a 100-example preview set. I'll use it to review the intended style before expanding the training data. The proposed distribution is:

| Conversation category | Target examples | Share |
|---|---:|---:|
| Natural casual conversation | 25 | 25% |
| Banter and roasting | 25 | 25% |
| Sarcasm and blunt wit | 20 | 20% |
| Dark humour and irony | 15 | 15% |
| Rants and conversational riffs | 10 | 10% |
| Sincere and warm tonal switches | 5 | 5% |
| **Total** | **100** | **100%** |

That mix is deliberate. If the dataset were made up entirely of jokes and insults, the model would have every reason to reach for that behaviour in every situation. Normal conversation needs just as much space, along with examples that show when it's better to be serious.

I also don't want to pad the dataset with hundreds of near-identical examples generated from a small handful of prompts. A bigger example count doesn't automatically mean the model is learning a wider range of conversational situations. The full dataset needs genuinely different scenarios, multi-turn exchanges, varied reply lengths, and plenty of cases where the right response is simply a straightforward answer.

This preview is for human review before I turn it into the final training set. I still need to check whether the examples are varied enough and whether the personality stays consistent without falling into repetitive habits.

## The Fine-Tuning Setup

For this iteration, I'm using **supervised fine-tuning (SFT)** with LoRA/QLoRA.

LoRA avoids updating every parameter in the original model. Instead, it adds a smaller set of trainable adapter weights, which makes experiments like this easier to manage. QLoRA builds on that approach by letting the base model load in quantized form during training.

Here's the planned setup for Persona v0.2:

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

For this run, I'm going back to the original instruct model instead of continuing from the previous experiment's adapter. The earlier dataset included quite a few template-heavy and relatively mild examples. If I carried that adapter forward, some of those habits might come with it, making it harder to tell whether Dataset2 is actually making a difference.

The two-epoch target is just where I'm starting for the comparison. I haven't decided that it's the ideal number. I'll look at the outputs and training behaviour before making any changes.

One other thing worth keeping in mind: this setup produces an adapter, not a standalone model. To use the trained LoRA adapter, I'll need to load it alongside the compatible Qwen2.5-3B-Instruct base model and tokenizer.

## The Part That's Giving Me Trouble

This is where I've been getting stuck.

I'm aiming for a less restrained, more uncensored-feeling conversational style, but I haven't quite worked out how to make it stick. No matter what I try, the model keeps drifting back toward what I can only call **generic Qwen behaviour**.

I can ask it to use a sharper voice, spell out the personality requirements in the prompt, and prepare examples that demonstrate the tone I'm after. Then I run a test, and that familiar assistant style creeps right back in.

It's becoming clear that describing a personality and teaching a model to express it are two different things. A prompt can tell a model to be blunt, sarcastic, spontaneous, or irreverent. Getting those traits to show up naturally across different conversations is much harder.

I'm not looking for profanity just for the sake of it, or for a model that confuses being rude with having a personality. I want a more convincing change in its conversational instincts: how it reacts, how it puts things, when it makes a joke, when it pushes back, and when it leaves the joke alone.

There are a few things I need to keep an eye on:

- **Personality strength:** Does the model genuinely sound sharper, or is it just dropping a joke into an otherwise generic answer?
- **Naturalness:** Does profanity or sarcasm fit the conversation, or does it show up simply because the dataset encourages it?
- **Variety:** Does the model change its rhythm and response structure, or keep recycling the same punchlines and patterns?
- **Context awareness:** Can it tell when teasing is welcome and when a sincere response matters more?
- **General usefulness:** Does the fine-tune preserve the base model's ability to follow instructions and answer practical questions?

There's also the risk of overfitting, especially with a small persona dataset. The model could start repeating particular phrases, overdoing one part of the personality, or losing some of the instruction-following ability that made the base model useful. A few entertaining generations won't be enough to convince me that the experiment has worked.

## What Comes Next?

For now, I'm continuing to work on Dataset2 and comparing its outputs with the original base model. I'll use the same prompts and generation settings for both, so I can make a fair side-by-side comparison instead of judging the results by a few hand-picked examples.

I'll be checking how strong and spontaneous the personality feels, whether the humour works, how much the responses vary, and whether the model can return to a sincere, helpful tone when the conversation calls for it. I'll also watch for any drop in its ability to follow ordinary instructions.

The model card is still being put together. I'll add the training metrics, final dataset composition, adapter details, and evaluation results once those parts are finished and I've checked them.

I'm not treating this as a finished release just yet. **Persona v0.2 “Teeth” is still experimental.**

Once I'm satisfied with the results, I'll publish the model and its model card on Hugging Face and add the link here. Until then, there's more dataset work ahead, along with a few more rounds of asking the model why it keeps slipping back into default-assistant mode. 😅

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
