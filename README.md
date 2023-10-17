# 25422_tls_bert
Fine-tuning pre-trained BERT model for detecting server TLS misconfiguration.

## Dependencies

Python 3.7+

## Setup

#### Clone Repo
```bash
git clone https://github.com/andrewcchu/25422_tls_bert.git
cd 225422_tls_bert
```

#### Setup Python Virtual Environment
NOTE: If different/needed, replace `python` with whatever your Python 3.7+ is aliased to (e.g., python3, python3.11, etc.)
```bash
python -m pip install virtualenv
virtualenv -p python venv
source venv/bin/activate
```

#### Install required packages
python -m pip install -r REQUIREMENTS.txt

#### Get model files
```bash
wget -O model.zip "https://uchicagoedu-my.sharepoint.com/:u:/g/personal/andrewcchu_uchicago_edu/EWmZeAEPBwxJnFpzUeGssRQBlCwldMsK9-8xGB77_u0ArQ?e=KHdfi6&download=1"
unzip model.zip
```

## Fine-Tuning

#### Data Description

The data in this repo represents parsed values of the header fields of packets in the TLS handshake sent back to the client by websites in the Tranco top list. Feel free to look at its format in `/dataset`. There are two subfolders here: `binary` and `top_multi`. `binary` labels all parsed handshakes as coming from either a "Properly Configured" or "Misconfigured" server. `top_multi` labels all "Misconfigured" handshakes from the binary set with a more specific reason for misconfiguration. Websites and labels for these datasets were determined using [`testssl.sh`](https://github.com/drwetter/testssl.sh).

#### Running Fine-tuning

Fine-tuning can be run variations on the following command:
```bash
python3.7 run_classifier.py \
--task_name=cola \
--do_train=true \
--do_eval=true \
--data_dir=./dataset/top_multi \
--vocab_file=./model/vocab.txt \
--bert_config_file=./model/bert_config.json \
--init_checkpoint=./model/model.ckpt-1000000 \
--max_seq_length=128 \
--train_batch_size=32 \
--learning_rate=2e-5 \
--num_train_epochs=10.0 \
--output_dir=./output/multi/top/top_multi_10_epochs_2e5_lr_128_max_32_bs_uncased \
--do_lower_case=True \
--save_checkpoints_steps 1000
```
