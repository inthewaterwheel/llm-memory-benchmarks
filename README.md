# Some LLM Memory Benchmarks

How much VRAM should you expect a transformer to use?

This code contains some benchmarks of various open-source OPT models, OPT 125m through 6.7b.

For a rule of thumb heuristic - it's `2-2.5 bytes per param`
This is somewhat intuitive given that these are FP16, and this is roughly the cost of storing the weights in VRAM alone.

| Model Params| 20 tokens  | 2000 tokens |
| ------------- | ------------- | ------------- |
| 125m | 258 MB  | 406 MB  |
| 350m | 667 MB  | 1.06 GB |
| 1.3b | 2.63 GB  | 3.42 GB  |
| 2.7b | 5.31 GB  | 6.64 GB  |
| 6.7b | 13.3 GB  | 15.44 GB  |

I didn't try larger models as they generally didn't fit on GPUs I have available.

## Instructions

- Launch a GPU instance via vast.ai or runpod.io, choose a device with >= 20 GB of VRAM
- Pick a Docker image with the appropriate Pytorch installed
- `pip install transformers`
- Run this notebook

## Discussion

### How do you run larger models?

You generally need a single machine with multiple GPUs, eg: [OPT-175B needs >400GB VRAM and only really works on machines with 8x 80GB cards](https://github.com/facebookresearch/metaseq/issues/146).

Your best bet for doing this would be to try to get GPUs from Lambda/Vast.ai/Runpod. These are kinda the "open"/"easy to access" GPU providers. 

GCP/AWS/Azure might work too - but they're more expensive & generally want you to contact a sales team to get access.

### What's the closest to SoTA that I can run on my consumer GPU? 

I generally doubt you can run anything close to SoTA locally anytime soon.

The Chinchilla paper suggested that you could get GPT-3 comparable results with ~70b params, but I'd guess GPT-3.5/GPT-4 have already used enough additional data to get to the point where ~175b+ params is worthwhile.

I'd guess that it might be nice to finetune a 350m to 3.7b param model for local usage for NLP tasks of yesteryear, ie: if it's something like classification, regression or ranking, the smaller models might have an adequate text understanding to do the job.

OpenAI's smallest two models are likely in the single-digit billions of parameter range, so take a look at their thoughts on [contexts where they'd be useful](https://beta.openai.com/docs/models/babbage).
