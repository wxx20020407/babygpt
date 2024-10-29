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
    我不是一个人，我们又只是一个太多，我看得我的衣服。这个大衣服又好久了，我要给我的小孩子，我不晓得。后去，我站在那儿。我们穿了长裙子，双手又穿着一大衣服，我也不肯放紧闭。我们不知道我在不能把握握住一对我。我不知道，是我的生命——之所以我们的生活。

    进了进宿舍时，我一边，在我们打电话，不肯讲什么，我们在找个月台湾，我讲什么东西，看得这样的人、脸还是极不知道的。

    “不过，我做了，我们在马德列阿尔，往今天梯子，我们去买好方便都好，坐在车头上等车，你们走路上，你去的车站了，我们一向你们走，我们就是不在他们、一样。”“真的，请你们去找，这些快点半点钟，在那儿去打电话，我还能听不欢喜的。”

    “算快的？”

    两人一面跑到太阳光下，那才开了，那张大步一抖，对面问我：“你在车上点多久！”

    我大笑，看到那个三毛去的一个空空空气里去。

     “还有，我还要走，你走了，你们走吧！”

    “我不走路，买了多年的人来了，你看了不行。”

     很少，也没有别人的同学习惯。

    “走吧，你放心，你就要走？”

    “走了！”

    “走吧，不是。”
---------------
```
## issues solved

1. The code specifies the use of a `float16` data representation to avoid reporting errors. This is because most microelectronics students' labs do not have cards superior to the V100 and therefore cannot support the bf16 data representation.
2. We have made improvements to adapt the Chinese language training. Because of the different token division, we use longer text for Chinese training. We adjusted the number of iterations and network structure in the config. At the same time, more attention head to determine the semantics of Chinese characters.
3. `always_save_checkpoint = True` This is a problem we found in training, where the Chinese training could not reach the target best_val_loss causing the program not to store ckpt.pt.
