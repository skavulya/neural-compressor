Weight-only quantization
===============

##  Prerequisite
```
# Installation
pip install -r requirements.txt
```

## Support status on HPU

Below is the current support status on Intel Gaudi AI Accelerator with PyTorch.

| woq_algo |   Status  |
|--------------|----------|
|   GPTQ   |  &#10004;|

> We validated the typical LLMs such as: `meta-llama/Llama-2-7b-hf`, `EleutherAI/gpt-j-6B`, `facebook/opt-125m`.

## Support status on CPU

Below is the current support status on Intel® Xeon® Scalable Processor with PyTorch.


| woq_algo |   status |
|--------------|----------|
|       RTN      |  &#10004;  |
|       GPTQ     |  &#10004;  |
|       AutoRound|  &#10004;  |
|       AWQ      |  &#10004;  |
|       TEQ      |  &#10004;  |

> We validated the typical LLMs such as: `meta-llama/Llama-2-7b-hf`, `EleutherAI/gpt-j-6B`, `facebook/opt-125m`.


## Run

`run_clm_no_trainer.py` quantizes the large language models using the dataset [NeelNanda/pile-10k](https://huggingface.co/datasets/NeelNanda/pile-10k) calibration and validates datasets accuracy provided by lm_eval, an example command is as follows.

### Quantization

```bash
python run_clm_no_trainer.py \
    --model meta-llama/Llama-2-7b-hf \
    --dataset NeelNanda/pile-10k \
    --quantize \
    --batch_size 8 \
    --woq_algo GPTQ \
    --woq_bits 4 \
    --woq_scheme asym \
    --woq_group_size 128 \
    --gptq_max_seq_length 2048 \
    --gptq_use_max_length \
    --output_dir saved_results
```
### Evaluation

```bash
# original model
python run_clm_no_trainer.py \
    --model meta-llama/Llama-2-7b-hf \
    --accuracy \
    --batch_size 8 \
    --tasks "lambada_openai,wikitext" \
    --output_dir saved_results

# quantized model
python run_clm_no_trainer.py \
    --model meta-llama/Llama-2-7b-hf \
    --load \
    --accuracy \
    --batch_size 8 \
    --tasks "lambada_openai,wikitext" \
    --output_dir saved_results
```

### Benchmark

```bash
# original model
python run_clm_no_trainer.py \
    --model meta-llama/Llama-2-7b-hf \
    --performance \
    --batch_size 8 \
    --output_dir saved_results

# quantized model
python run_clm_no_trainer.py \
    --model meta-llama/Llama-2-7b-hf \
    --load \
    --performance \
    --batch_size 8 \
    --output_dir saved_results
```

For more information about parameter usage, please refer to [PT_WeightOnlyQuant.md](https://github.com/intel/neural-compressor/blob/master/docs/source/3x/PT_WeightOnlyQuant.md)
