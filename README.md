Deep Character-Level Neural Machine Translation
============
We implement a **Deep Character-Level Neural Machine Translation By Learning Morphology** based on [Theano](https://github.com/Theano/Theano) and [Blocks](https://github.com/mila-udem/blocks). Please intall relative packages according to [Blocks](http://blocks.readthedocs.io/en/latest/setup.html) before testing our program. Note that, please use Python 3 instead of Python 2. There will be some problems with Python 2. 

It is an improved version of [DCNMT](https://github.com/swordyork/dcnmt/tree/old-version), the architecture of DCNMT is shown in the following figure which is a single, large neural network. Please refer to the paper for the details.
![DCNMT](/dcnmt.png?raw=true "The architecture of DCNMT")


Training
-----------------------
If you want to train your own model, please prepare a parallel linguistics corpus, like corpus in [WMT](http://www.statmt.org/wmt15/translation-task.html). A GPU with 12GB memory will be helpful. You could run `bash train.sh` or follow these steps.
 1. Download the relative scripts (tokenizer.perl, multi-bleu.perl) and nonbreaking_prefix from [mose_git](https://raw.githubusercontent.com/moses-smt/mosesdecoder/master/scripts).
 2. Download the datasets, then tokenize and shuffle the cropus.
 3. Create the character list for both language using `create_vocab.py` in `preprocess` folder. Don't forget to pass the language setting, vocabulary size and file name to this script.
 4. Create a `data` folder, and put the `vocab.*.*.pkl` and `*.shuf` in the `data` folder.
 5. Prepare the tokenized test set, and put them in `data` folder.
 6. Edit the `configurations.py`, and run `python training_adam.py`. It will take 1 to 2 weeks to train a good model.

You need to decrease the learning rate during training, or set the learning rate to 1e-4 which may result a longer training time. To save training time, you may need to perform validation on other computers manually or use a script. We will dump the model every 20,000 updates by default. For example, when the model is trained after 800,000 updates, you could run `python testing.py dcnmt_en2cs_800000` to validate the performance.


Testing
-----------------------
We have trained several models which listed in the following table. However, because of the limitation of available GPU and long training time (two weeks or more), we don't have enough time and resource to train on more language pairs. If you run into any trouble, please open an issue or email me directly at `echo c3dvcmQueW9ya0BnbWFpbC5jb20K | base64 -d`. Thanks!

| language pair | dataset | BLEU_dev | BLEU_test1 | BLEU_test2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|
| en-cs | [wmt15](http://www.statmt.org/wmt15/translation-task.html) | 17.89 | 18.60 | 16.96 |

These models are evaluated on `newstest2014 (BLEU_test1)` and `newtest2015 (BLEU_test2)` using the best validation model on `newstest2013 (BLEU_dev)`. You can download these models from [dropbox](https://www.dropbox.com/sh/o34kd051rm2duvu/AAA5ReaXrr043EocIYZuwPJsa?dl=0), then put them (dcnmt_\*, data, configurations.py) in this directory. To perform testing, run `python testing.py dcnmt_en2cs_800000` or other corresponding language pairs. It takes about an hour to do translation on 3000 sentences if you have a moderate GPU.

Updating...
