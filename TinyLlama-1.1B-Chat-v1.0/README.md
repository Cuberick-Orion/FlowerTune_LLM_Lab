---
base_model: TinyLlama/TinyLlama-1.1B-Chat-v1.0
library_name: peft
---

## Model Details

Our method is based on `TinyLlama/TinyLlama-1.1B-Chat-v1.0`.

## How to Get Started with the Model

First, set up the enviroment following the main [README.md](../README.md) file.

Then use the code below to get started with the model.

`flwr run .`

## Training Details

### Training Data

We train with the default supplied data as:

```toml
dataset.name = "flwrlabs/code-alpaca-20k"
```

#### Training Hyperparameters

```toml
model.name = "TinyLlama/TinyLlama-1.1B-Chat-v1.0"
model.quantization = 4
model.gradient-checkpointing = true
model.lora.peft-lora-r = 8
model.lora.peft-lora-alpha = 16 
train.save-every-round = 5
train.learning-rate-max = 5e-5
train.learning-rate-min = 1e-6
train.seq-length = 512
train.training-arguments.output-dir = ""
train.training-arguments.learning-rate = ""
train.training-arguments.per-device-train-batch-size = 16 
train.training-arguments.gradient-accumulation-steps = 1
train.training-arguments.logging-steps = 10
train.training-arguments.num-train-epochs = 3
train.training-arguments.max-steps = 10
train.training-arguments.save-steps = 1000
train.training-arguments.save-total-limit = 10
train.training-arguments.gradient-checkpointing = true
train.training-arguments.lr-scheduler-type = "constant"
strategy.fraction-fit = 0.2
strategy.fraction-evaluate = 0.0
num-server-rounds = 200
```

#### Communication Cost

3448 MB

## Evaluation

<!-- This section describes the evaluation protocols and provides the results. -->
Download the checkpoints at [this link](https://drive.google.com/drive/folders/19qr-Fsf1JBT_a0IPG0mAyHTtBBfOioJQ?usp=sharing).

### Procedures

See [this](https://github.com/adap/flower/tree/main/benchmarks/flowertune-llm/evaluation/code) for downloading the necessary packages and eval script. Below, we provide the commands to run the evaluations on each metric respectively.

For `TinyLlama/TinyLlama-1.1B-Chat-v1.0` results:

```bash
# humaneval
# pass@1": 0.12195121951219512
python main.py \
--model=TinyLlama/TinyLlama-1.1B-Chat-v1.0 \
--peft_model=path_to_the_model/peft_200  \
--max_length_generation=1024  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=humaneval \
--metric_output_path=./tinyllama1.1b/evaluation_results_humaneval.json

# mbpp
#    "pass@1": 0.026
python main.py \
--model=TinyLlama/TinyLlama-1.1B-Chat-v1.0 \
--peft_model=path_to_the_model/peft_200  \
--max_length_generation=2048  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=mbpp \
--metric_output_path=./tinyllama1.1b/evaluation_results_mbpp.json


# multiple-js
# "pass@1": 0.09937888198757763
python main.py \
--model=TinyLlama/TinyLlama-1.1B-Chat-v1.0 \
--peft_model=path_to_the_model/peft_200  \
--max_length_generation=1024  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=multiple-js \
--metric_output_path=./tinyllama1.1b/evaluation_results_multiple_js.json


# multiple-cpp
# "pass@1": 0.09937888198757763
python main.py \
--model=TinyLlama/TinyLlama-1.1B-Chat-v1.0 \
--peft_model=path_to_the_model/peft_200  \
--max_length_generation=1024  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=multiple-cpp \
--metric_output_path=./tinyllama1.1b/evaluation_results_multiple_cpp.json
```

### Results

__Average__: 8.67

__MBPP__: 2.60

__HumanEval__: 12.20

__MultiPL-E (JS)__: 9.94

__MultiPL-E (C++)__: 9.94


### Framework versions

- PEFT 0.14.0