# babygpt
Fudan AI-design course midterm project by Xiaoxing Wu.

## install

```
pip install torch numpy transformers datasets tiktoken wandb tqdm
```

Dependencies:

- [pytorch](https://pytorch.org) <3
- [numpy](https://numpy.org/install/) <3
-  `transformers` for huggingface transformers <3 (to load GPT-2 checkpoints)
-  `datasets` for huggingface datasets <3 (if you want to download + preprocess OpenWebText)
-  `tiktoken` for OpenAI's fast BPE code <3
-  `wandb` for optional logging <3
-  `tqdm` for progress bars <3

## quick start

This is a modified and trained model based on nanoGPT. It can generate Chinese sentences based on the famous prose studied, in input.txt in data, or directly modify the url in the code to download it from the internet. 

First, use prepared.py to set up `itos` and `stoi`, and also generates `train.bin` and `val.bin` in that data directory.

```sh
python data/chinese/prepare.py
```

This creates a `train.bin` and `val.bin` in that data directory. Now it is time to train your GPT. The size of it very much depends on the computational resources of your system:
