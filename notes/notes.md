## Project Introduction

The purpose of this voice note is to provide context for the development of this project. It's one of those accessory components that I want to have because I want to use them, and I haven't seen it created yet. I would be very happy to be proven wrong and be able to use someone else's technology for this. If not, I will work on this as my own project. It will be open-sourced, as most of the things that I'm working on these days are, especially utilities that might be useful for other people doing open-source development.

## The Problem: Disorganized AI Models

Here's the elevator pitch: anyone using local AI models is familiar with the problem of pulling very large model weights from Hugging Face over and over again. I think there's actually a similarity between MCP (Minecraft Protocol), where the problem is that every MCP tool wants to do MCP in its own way and has its own storage. To an extent, models are kind of like that too, in the sense that you might find that you download something from Hugging Face, and it uses a cache on your computer. Then, you use another program for voice tech, and for example, if I use Kdenlive, it has a subtitle generation thing, and that will pull Whisper.

The issue, it took me a while to really wrap my head around Python environments and how the stacks fit together. It occurred to me recently, and I hope I'm not wrong about this, but I'm pretty sure that this is accurate to say, that we might have, let's say, Whisper. Kdenlive might need its own environment, but the Whisper weights can be shared between different environments.

It's not that computer storage is at such a premium that you don't want to download Whisper multiple times. It's more that it becomes a waste of time when you're working in, you might be working in ComfyUI, and Ollama, and LM Studio, and different tools, to each time either be bringing your models into their thing and not knowing exactly where they are. It does, I have seen it becoming an issue whereby it started with Ollama. Then I was asking ChatGPT, I said, "Hang on, we just pulled 50 gigabytes of models from the internet. That's a lot of data. Where does that go that I can use it and see what I have?" It kind of by default puts it in its own place in the system.

## The Proposed Solution: An AI Model Manager

To cut to the chase, here's what I would like to have, and this is the goal to create it. I'd like to have an AI model manager that is a local program with a GUI. I think it's better to have a GUI than a web UI, but it could be implemented either way. The idea here would just be to have a single source of truth for "this is my model store."

## Why a Centralized Manager is Needed

The reason that I'm thinking about prototyping this is that what I've noticed is there's been a couple of CLI's to support model downloading, but they tend to be for specific tools. For example, you might find that people working with generative AI have tools figured out for pulling the Quant models. I suspect, like a lot of people, I work across these different tools. So I might need to use Whisper, and then fine-tuned Whisper, and then Ollama, and then I'm using Fish Speech, and Kakoro, and TTS. I have models across different categories, and I just need one place to keep myself sane and know every time I'm trying a new tool, this is where they live, and it's organized just according to the type of model that it is. So I might have my image generation, LoRAs, ControlNets, etc. Even if you're running stuff in Docker on local, it doesn't really matter because everything can point, you can mount a Docker container to a Docker volume to a host. So I personally think that there is a significant advantage in just, there would be significant advantage in having a centralized place on your system where that's where your AI models are, that's where, and you can not go crazy trying to remember where did this put that. If a tool doesn't support an external model library, then there's nothing that can be done about that. But most tools, especially with stuff like GGUF, there's a significant degree of interoperability.

I don't think it's that tools necessarily want to put up obstacles, saying, "We don't want you, you should put it in the default store." I think it's often just that there isn't really tooling. So that's the idea.

## Categorization of Models

To go from abstract to concrete, Hugging Face token support so that the user can put it in. The other thing that I do want is I have some fine-tunes of Whisper, and I have some LoRAs. So that's within, so I think it has to be a category-based system. I'm probably going to be missing some categories, but the big ones would be large language models, embedding models, diffusion models, image generation accessories, because there's a bunch of little models that kind of, and I think the way ComfyUI divides their model store is very logical. In fact, it might be very easy to just try to replicate that. Then you have speech-to-text models, speech-to-text fine-tunes, text-to-speech models (TTS).

Then there's, those would be the big categories, but then you'd also have smaller categories like punctuation restoration, niche models like there's one in Hebrew called for diacritic restoration, which is restoring vocalization. So there is actually a gigantic range of models. But I think because it would be impossible to come up with an absolutely complete, oh, I forgot one class, which is multimodal models. A very important one actually, because I'm using those locally quite a lot at the moment. So, as I was about to say, there are many more subcategories that could be considered than that, but that would be just a good start for a categorization system.

## Implementation Details

I think the best practice in, that I'm familiar with in Linux at least, is to define a base path. So the user should be able to define the base path as that's where their model store. I happen to use `home folder/AI/models`, but the user can pick however they like to do it.

Then, setting up subfolders. Probably I think the most robust way to do it actually would be with a lightweight database, SQLite. Then just defining the base folders for these different types. So a base folder for large language models, and tags for variants, for example, GGUF, quantizations, fine-tunes as a tag.

Something I've seen, there's a couple of great MCP tools. I'm using one at the moment called MCP Hub, and it's absolutely brilliant. I mean, it's essentially just creating a taxonomy for this problem of MCPs are great, but how do you actually group them up into things? So I think the idea here is mostly just organizational, like providing an admin interface for the user to say, and that's why I just think a basic taxonomy for categories would be helpful.

## The End Objective

The end objective is that the user, let's say me, someone comes out with this great new local AI tool that can use large language models. You set it up, and it then tries to get you into this download from our registry thing or point it to your own path. That's it. You have your, you know exactly where your models are. They're managed properly. You can go into the UI if you want and see what you have. You can delete what you don't need. Maybe down the line there could be duplicate flagging, which I'm sure a lot of people also end up using different tools and then inadvertently forgetting they already had Whisper or. In fact, and there's actually a good, this would be very useful in that respect because models are very big. I tend to be pretty diligent about pruning. If Quant 3 comes out, and I don't need to keep Quant 2.5, I'll just delete it and free up like 20 or 30, potentially 20-30 gigs. Again, it's not really coming from an economizing standpoint. It's just it gets very confusing when you try to connect a model to a tool, and then you have this overwhelming list of everything you tried out. So it's more like keeping it in good order.

## Future Possibilities: AI Assistant

Ironically, this would actually be perfect for AI itself. There could be a little local agent living in this model manager. That's your sidekick for saying, that I could chat with it saying, "Hey, I'm pretty sure I have way too many models in my store, and old stuff, and overlapping stuff. Can you clean it up?" Ask my permission before deleting stuff, but otherwise, give me recommendations. That would be very feasible, I think.

## UI Preference and Conclusion

That's kind of the spec, basically. I'm not sure whether this makes more sense to do as a web UI, as I said, or as a desktop program. I tend to prefer desktop programs because I guess they're just easier to use. They're always there. You just open them. It could be either. I'm on Linux, as you know. I'm addressing Claude for anyone who happens to listen to this. Let's start to implement.