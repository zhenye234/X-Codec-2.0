[![arXiv](https://img.shields.io/badge/arXiv-Paper-<COLOR>.svg)](https://arxiv.org/abs/2502.04128)
 
# X-Codec-2.0
Paper: LLaSA: Scaling Train-time and Inference-time Compute for LLaMA-based Speech Synthesis  

**Update (2026-06-25):** Xcodec2 has been added to Transformers! A Transformers-native checkpoint can be found been [here](https://huggingface.co/HKUSTAudio/xcodec2-hf)!

**Update (2025-02-13):** Add [Llasa finetune instruction](https://github.com/zhenye234/LLaSA_training/tree/main/finetune).

**Update (2025-02-07):** Our paper has been released!


## Directly used on Hugging Face

**Codec**: [xcodec2](https://huggingface.co/HKUST-Audio/xcodec2) (Use `xcodec2==0.1.5` for codec inference and llasa fine-tuning. I’ve removed unnecessary dependencies, and it works fine in my testing. However,  I’m not sure if other problems may arise. If you prefer more stability, I recommend using `xcodec2==0.1.3` which accurately aligns during my codec training.)
 

**Llasa-collections**: [Llasa-collections](https://huggingface.co/collections/HKUSTAudio/llasa-679b87dbd06ac556cc0e0f44)

## Features

- **Single Vector Quantization**
  - 65536 Codebook Size using Finite Scalar Quantization achieving 99% codebook usage. ( comparable to text tokenizers, LLaMA3 128256)
  - 50x1 Tokens per Second

- **Multilingual Speech Semantic Support**
  - Uses Wav2Vec2-BERT, a semantic encoder pre-trained on 4.5M hours of unlabeled audio data covering more than 143 languages.
  - Codec trained on 150k hours of multilingual speech data, including Emilia (En/Zh/De/Fr/Ja/Ko) and MLS (En/Fr/De/Nl/Es/It/Pt/Pl).

- **High-Quality Speech Reconstruction**
  - Transformer + Vocos Decoder
  - BigCodec encoder
  - Spec discriminator with FFT sizes {78, 126, 206, 334, 542, 876, 1418, 2296} tailored for transformer decoder. [Details here](https://openreview.net/pdf?id=4YpMrGfldX)
  - Achieving UTMOS 4.13 WER 2.47 (hubert-large-ls960-ft)  sim 0.82 (wavlm_large_finetune) stoi 0.92  pesq-nb 3.05  pesq-wb 2.44 on librispeech-test-clean reconstruction (gt: WER 1.96 UTMOS 4.09)
  - Only for 16kHz speech


##  Commandline Usage
## Setup
Code is tested on `python3.9`

Please follow the following steps to setup your environment
1. Clone this repo
2. conda create --name xcodec2 python=3.9 
3. conda activate xcodec2  
2. `pip install -r requirements.txt`
3. [Download the pretrained checkpoint here](https://huggingface.co/HKUST-Audio/xcodec2/blob/main/ckpt/epoch%3D4-step%3D1400000.ckpt)


## Inference
```bash
python inference.py  
```
 
## Train
To train a XCodec2, firstly you have to prepare your data 

1. Make a file list by:
```bash
python get_tsv.py
```

2. Train a X-Codec-2.0 with the default setting by:

```bash
python train.py log_dir=/path/to/log_dir
```

## Large-scale training, Batch inference and large-scale code extracting:

Batch inference
```bash
python inference_save_code.py
```
Training
```bash
Sbatch train_slurm.sh
```

Code extracting
```bash
Sbatch large_scale_save_code.sh
```

Code will save in output folder with the same subfolder structure for audio file.


 
## Acknowledgement
I would like to extend a special thanks to authors of BigCodec, since our code base is mainly borrowed from  [BigCodec](https://github.com/Aria-K-Alethia/BigCodec).
