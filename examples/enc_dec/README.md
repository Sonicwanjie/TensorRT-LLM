# Encoder-Decoder

This document shows how to build and run an Encoder-Decoder (Enc-Dec) model in TensorRT-LLM on NVIDIA GPUs.

## Overview

The TensorRT-LLM Enc-Dec implementation can be found in [tensorrt_llm/models/enc_dec/model.py](../../tensorrt_llm/models/enc_dec/model.py). The TensorRT-LLM Enc-Dec example code is located in [`examples/enc_dec`](./). There are two main files in that folder:

 * [`build.py`](./build.py) to build the [TensorRT](https://developer.nvidia.com/tensorrt) engine(s) needed to run the Enc-Dec model,
 * [`run.py`](./run.py) to run the inference on an input text.

## Usage

The TensorRT-LLM Enc-Dec example code locates at [examples/enc_dec](./). It takes HF weights as input, and builds the corresponding TensorRT engines. For single GPU, there will be two TensorRT engines, one for Encoder and one for Decoder.

## Encoder-Decoder Model Support
- [T5](https://huggingface.co/docs/transformers/main/en/model_doc/t5)

In this example, we use T5 to showcase TRT-LLM support on Enc-Dec models.

### Build TensorRT engine(s)

Need to prepare the HuggingFace T5 checkpoint first by following the guides here https://huggingface.co/docs/transformers/main/en/model_doc/t5.

TensorRT-LLM Enc-Dec builds TensorRT engine(s) from HF checkpoint. For the first time of running this example, user needs to download the T5 model ckpt from HF. After obtaining the HF T5 ckpt, user can build the TensorRT engines.

```bash
# download t5-small ckpt to ./models (one-time)
python download.py

# Build t5-small using a single GPU and FP16, supporting beam search up to 3 beam_width
python build.py --model_dir ./models/ \
                --use_bert_attention_plugin \
                --use_gpt_attention_plugin \
                --dtype float16 \
                --max_beam_width 3
# build.py will by default save the TRT engines into ./trt_engines
```

### Run

To run a TensorRT-LLM Enc-Dec model using the engines generated by build.py

```bash
# Run inference with beam search
python3 run.py --max_new_token=64 --num_beams=3
```