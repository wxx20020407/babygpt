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
torch.Size([1, 503])
我喜欢的《杂花图长卷》。他的生命奔泻出淋漓而又逦泼的墨色与线条，躁动的笔墨后面游动着不驯和无奈。在这里，仅说笔墨趣味就很不够了，仅说气韵生动也太矜持了。

对徐渭我了解得比较多。从小在乡间老人口中经常听“徐文长”的故事，年长后细读了他的全部文集，洗去了有关他的许多不经传说，而对他的印象却愈来愈深。他实在是一个才华横溢、具有充分国际可比性的大艺术家，但人间苦难也真是遇过复杂的家庭变故，参加过抗倭斗争，又曾惶恐于政治牵连。他曾自撰墓志铭，九次自杀而未死。他还误杀过妻子，坐过六年多监狱。他厌弃人世、厌弃家庭、厌弃自身，但他又多么清楚自己在文化艺术史上的千古重量，这就产生了特别残酷、也特别响亮的生命冲撞。浙江的老百姓凭着直觉感触到了他的生命温度，把他作为几百年的谈资。老百姓主要截取了他倦狂的一面来作滑稽意义上的衍伸，而实际上他的佯狂背后埋藏的都是悲剧性的激潮。在中国古代画家中，人生经历像徐渭这样凄厉的人不多，即使有，也没有能力把它幻化为一幅幅生命本体悲剧的色彩和线条。

明确延续着这种在中国绘画史上很少见到的强烈悲剧意识的，便是朱耷。他具体的遭遇没有徐渭那样惨，但作为已亡的大明皇室的后裔，他的悲剧性
---------------
```

```sh
---------------
torch.Size([1, 506])
    我不是一个人，有一天我的小朋友来信，我来信的时候，我也不会知道是怎么来的，我们就是我受到我的爱，请你去沙漠里，荷西，为什么你在教他，我去沙漠里?我不肯去信，我在那里，我不知道，你在沙漠里——以后，请你向沙漠去，请你多漠帐。我自在不再去，只是不送你的地址。"我一见法蒂玛的两个字，一个字不会回来了，我说："没有信在我的信里，你不太写信时，我的书上，快来。"荷西好嘛!罕地，沙哈拉威人有时才跑出来。"我惊叫的不肯，这封信给我轻轻的好似的通信，我回到沙漠里，我一听说："可以跑到沙漠里，要去沙漠"，给他。"你去沙漠之后，给你们的信，明天的时候我们再一面请求照片?"沙漠里。

　　结婚后，荷西结婚了。

　　荷西的信来说，你的沙漠里没有录音色、出来，再上先去荷西在写信，你看了很大的秘密，从来没有回西班牙文信。

　　荷西的朋友，我这信上将卷里一封信，带回信箱子来了，要我们开始写信，再找我。"

　　我在乱帐纸上，我们写信，为他拍了衣服，将我的身上放出去，不知道我们没有沙漠人，才写信来信，我要问他："我想明天，就在这里请你们住些帐！"荷西从此地的房间，就是荷西，这个时候我们都在家里住下。

　　荷西被荷西找了过了
---------------
```
## issues solved

1. The code specifies the use of a `float16` data representation to avoid reporting errors. This is because most microelectronics students' labs do not have cards superior to the V100 and therefore cannot support the bf16 data representation.
2. We have made improvements to adapt the Chinese language training. Because of the different token division, we use longer text for Chinese training. We adjusted the number of iterations and network structure in the config. At the same time, more attention head to determine the semantics of Chinese characters.
3. `always_save_checkpoint = True` This is a problem we found in training, where the Chinese training could not reach the target best_val_loss causing the program not to store ckpt.pt.
