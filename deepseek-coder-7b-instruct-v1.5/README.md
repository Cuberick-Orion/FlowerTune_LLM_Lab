---
base_model: deepseek-ai/deepseek-coder-7b-instruct-v1.5
library_name: peft
---

## Model Details

Our method is based on `deepseek-ai/deepseek-coder-7b-instruct-v1.5`.

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
model.name = "deepseek-ai/deepseek-coder-7b-instruct-v1.5"
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
train.training-arguments.per-device-train-batch-size = 16 # 16
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
num-server-rounds = 50 
```

#### Communication Cost

50 Rounds, 3.00 GiB

## Evaluation

<!-- This section describes the evaluation protocols and provides the results. -->
Download the checkpoints at [this link](https://drive.google.com/drive/folders/1EYTVdWQB-QMNeACqwk8Dlna8KuHUKi2J?usp=sharing). Or navigate to `./results/peft_50` to obtain the checkpoints.

### Procedures

See [this](https://github.com/adap/flower/tree/main/benchmarks/flowertune-llm/evaluation/code) for downloading the necessary packages and eval script. Below, we provide the commands to run the evaluations on each metric respectively.

For `deepseek-coder-7b-instruct-v1.5` results:

```bash
# humaneval
#   "pass@1": 0.6463414634146342
python main.py \
--model=deepseek-ai/deepseek-coder-7b-instruct-v1.5 \
--peft_model=path_to_the_model/peft_50  \
--max_length_generation=1024  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=humaneval \
--metric_output_path=./deepseek-coder-7b/evaluation_results_humaneval.json

# mbpp
# pass@1": 0.568
python main.py \
--model=deepseek-ai/deepseek-coder-7b-instruct-v1.5 \
--peft_model=path_to_the_model/peft_50  \
--max_length_generation=2048  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=mbpp \
--metric_output_path=./deepseek-coder-7b/evaluation_results_mbpp.json

# multiple-js
# "pass@1": 0.5590062111801242 
python main.py \
--model=deepseek-ai/deepseek-coder-7b-instruct-v1.5 \
--peft_model=path_to_the_model/peft_50  \
--max_length_generation=1024  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=multiple-js \
--metric_output_path=./deepseek-coder-7b/evaluation_results_multiple_js.json

# multiple-cpp
# "pass@1": 0.577639751552795
python main.py \
--model=deepseek-ai/deepseek-coder-7b-instruct-v1.5 \
--peft_model=path_to_the_model/peft_50  \
--max_length_generation=1024  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=multiple-cpp \
--metric_output_path=./deepseek-coder-7b/evaluation_results_multiple_cpp.json
```

### Results

__Average__: 58.77

__MBPP__: 56.80

__HumanEval__: 64.63

__MultiPL-E (JS)__: 55.90

__MultiPL-E (C++)__: 57.76


### Framework versions

- PEFT 0.14.0