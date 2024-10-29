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

Second, train the model. Parameters such as `n_layer`, `n_head`, `n_embd`, `max_iters` can be adjusted in config/train_chinese.py based on hardware performance.

```sh
python train.py config/train_chinese.py
```

Third, let the model output some examples. If the start parameter is not given, the code internally defaults to `\n` as a hint.

```sh
python sample.py --out_dir=out-chinese [--start=something]
```

Now you'll get several samples at the length of 500 characters.

## demo

Try to use our pretrained model to generate some sentences!

Make sure the 'ckpt.pt' properly downloaded in the right directory.

```sh
ls out-chinese/ckpt.pt 
```

If the file does exists, continue:

```sh
python sample.py --out_dir=out-chinese
```

It will generate samples like:

```sh
---------------
torch.Size([1, 506])
我不是一个人，怕只能说战争的只是，而不是自己的内心却想早死。王安石很奇怪，问他为什么，他说：“我先死，您就会给我写墓志铭，好流传后世了。”王安石一听就掂出了这个人的人格重量，不再理会。有一个叫李师中的小人水平更高一点，在王安石推行新法而引起朝廷上下非议纷纷的时候，他写了长长的十篇《巷议》，说街头巷尾都在说新法好，宰相好。本来这对王安石是雪中送炭般的支持，但王安石一眼就看出了《巷议》的伪诈成分，开始提防他。只有象管仲、王安石这样，小人们所布下的情感迷魂阵才能破除，但对很多人物来说，几句好话一听心肠就软，小人要俘虏他们易如反掌。

第三，心态上的恐惧。小人和善良人们往往有一段或短或长的情谊上的“蜜月期”，当人们开始有所识破的时候，小人的耍泼期也就来到了。平心而论，对于小人的耍泼，多数人是害怕的。小人不管实际上胆子多小，耍起泼来有一种玩命的外相。好人虽然不见得都怕死，但要死也死在战争、抢险或与匪徒的格斗中，与小人玩命，他先泼你一身脏水，把事非颠倒得让你成为他的同类，就像拉进一个泥潭翻滚得谁的面目也看不清，这样的死法多窝囊！因此，小人们用他们的肮脏，摆开了一个比世界上任何真正的战场都令人恐怖的混乱方阵，使
---------------
```
## issues solved

1. The code specifies the use of a `float16` data representation to avoid reporting errors. This is because most microelectronics students' labs do not have cards superior to the V100 and therefore cannot support the bf16 data representation.
2. We have made improvements to adapt the Chinese language training. Because of the different token division, we use longer text for Chinese training. We adjusted the number of iterations and network structure in the config. At the same time, more attention head to determine the semantics of Chinese characters.
3. `always_save_checkpoint = True` This is a problem we found in training, where the Chinese training could not reach the target best_val_loss causing the program not to store ckpt.pt.
