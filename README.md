# News Recommendation

The repository currently includes the following models.

| Model     | Full name                                                                 | Paper                                              |
| --------- | ------------------------------------------------------------------------- | -------------------------------------------------- |
| NRMS      | Neural News Recommendation with Multi-Head Self-Attention                 | https://www.aclweb.org/anthology/D19-1671/         |
| NAML      | Neural News Recommendation with Attentive Multi-View Learning             | https://arxiv.org/abs/1907.05576                   |
| LSTUR     | Neural News Recommendation with Long- and Short-term User Representations | https://www.aclweb.org/anthology/P19-1033.pdf      |
| DKN       | Deep Knowledge-Aware Network for News Recommendation                      | https://dl.acm.org/doi/abs/10.1145/3178876.3186175 |
| TANR      | Neural News Recommendation with Topic-Aware News Representation           | https://www.aclweb.org/anthology/P19-1110.pdf      |

## Get started

### Install the package

Install from <https://pypi.org/>.

```bash
pip install news-recommendation
```

Or install it manually.

```bash
git clone https://github.com/yusanshi/news-recommendation.git
cd news-recommendation
pip install .
```

### Download and process the datasets

Create an empty directory as our working directory.
```bash
cd ~
mkdir whatever-name-you-like && cd "$_"
export ROOT_DIRECTORY=`pwd`
```

Download and unzip GloVe pre-trained word embedding.

```bash
cd $ROOT_DIRECTORY
mkdir -p data/raw/glove && cd "$_"
wget https://nlp.stanford.edu/data/glove.840B.300d.zip
sudo apt install unzip
unzip glove.840B.300d.zip
rm glove.840B.300d.zip
```

Download and process MIND-small dataset. Note MIND Small doesn't have a test set, so we just copy the validation set as test set :)

```bash
cd $ROOT_DIRECTORY
mkdir -p data/raw/mind-small && cd "$_"
wget https://mind201910small.blob.core.windows.net/release/MINDsmall_train.zip \
 https://mind201910small.blob.core.windows.net/release/MINDsmall_dev.zip
unzip MINDsmall_train.zip -d train
unzip MINDsmall_dev.zip -d val
cp -r val test # MIND Small has no test set :)
rm MINDsmall_*.zip

cd $ROOT_DIRECTORY
python -m news_recommendation.data_preprocess --source_dir=$ROOT_DIRECTORY/data/raw/mind-small \
 --target_dir=$ROOT_DIRECTORY/data/mind-small \
 --dateset=mind-small \
 --glove_path=$ROOT_DIRECTORY/data/raw/glove/glove.840B.300d.txt
```

Download and process MIND-large dataset. Note MIND Large test set doesn't have labels, see #11.
```bash
cd $ROOT_DIRECTORY
mkdir -p data/raw/mind-large && cd "$_"
wget https://mind201910small.blob.core.windows.net/release/MINDlarge_train.zip \
 https://mind201910small.blob.core.windows.net/release/MINDlarge_dev.zip \
 https://mind201910small.blob.core.windows.net/release/MINDlarge_test.zip
unzip MINDlarge_train.zip -d train
unzip MINDlarge_dev.zip -d val
unzip MINDlarge_test.zip -d test
rm MINDlarge_*.zip

cd $ROOT_DIRECTORY
python -m news_recommendation.data_preprocess --source_dir=$ROOT_DIRECTORY/data/raw/mind-large \
 --target_dir=$ROOT_DIRECTORY/data/mind-large \
 --dateset=mind-large \
 --glove_path=$ROOT_DIRECTORY/data/raw/glove/glove.840B.300d.txt
```

Download and process Adressa 1week dataset.
```bash
cd $ROOT_DIRECTORY
mkdir -p data/raw/adressa-1week && cd "$_"
wget ...


cd $ROOT_DIRECTORY
python -m news_recommendation.data_preprocess --source_dir=$ROOT_DIRECTORY/data/raw/adressa-1week \
 --target_dir=$ROOT_DIRECTORY/data/adressa-1week \
 --dateset=adressa-1week
```


Run.

```bash
# Train and save checkpoint into `checkpoint/{model_name}/` directory
python -m news_recommendation.train
# Load latest checkpoint and evaluate on the test set
python -m news_recommendation.evaluate
```

You can visualize metrics with TensorBoard.

```bash
tensorboard --logdir=runs

# or
tensorboard --logdir=runs/{model_name}
# for a specific model
```

> Tip: by adding `REMARK` environment variable, you can make the runs name in TensorBoard more meaningful. For example, `REMARK=num-filters-300-window-size-5 python -m news_recommendation.train`.

## Results

| Model     | AUC | MRR | nDCG@5 | nDCG@10 | Remark |
| --------- | --- | --- | ------ | ------- | ------ |
| NRMS      |     |     |        |         |        |
| NAML      |     |     |        |         |        |
| LSTUR     |     |     |        |         |        |
| DKN       |     |     |        |         |        |
| TANR      |     |     |        |         |        |

Checkpoints: <https://drive.google.com/open?id=TODO>

You can verify the results by simply downloading them and running `MODEL_NAME=XXXX python -m news_recommendation.evaluate`.

## Credits

- Dataset by **MI**crosoft **N**ews **D**ataset (MIND), see <https://msnews.github.io/>.
