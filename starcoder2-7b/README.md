---
base_model: bigcode/starcoder2-7b
library_name: peft
---

## Model Details

Our method is based on `bigcode/starcoder2-7b`.

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
model.name = "bigcode/starcoder2-7b"
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
num-server-rounds = 100 
```

#### Communication Cost

36406 MB

## Evaluation

<!-- This section describes the evaluation protocols and provides the results. -->
Download the checkpoints at [this link](https://drive.google.com/drive/folders/1hycDYKiJm2kc963OISyerCjV-kcix9Z3?usp=sharing).

### Procedures

See [this](https://github.com/adap/flower/tree/main/benchmarks/flowertune-llm/evaluation/code) for downloading the necessary packages and eval script. Below, we provide the commands to run the evaluations on each metric respectively.

For `bigcode/starcoder2-7b` results:

```bash
# humaneval
python main.py \
--model=bigcode/starcoder2-7b \
--peft_model=path_to_the_model/peft_50  \
--max_length_generation=1024  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=humaneval \
--metric_output_path=./bigcode/starcoder2-7b/evaluation_results_humaneval.json

# mbpp
python main.py \
--model=bigcode/starcoder2-7b \
--peft_model=path_to_the_model/peft_50  \
--max_length_generation=2048  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=mbpp \
--metric_output_path=./bigcode/starcoder2-7b/evaluation_results_mbpp.json

# multiple-js
python main.py \
--model=bigcode/starcoder2-7b \
--peft_model=path_to_the_model/peft_50  \
--max_length_generation=1024  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=multiple-js \
--metric_output_path=./bigcode/starcoder2-7b/evaluation_results_multiple_js.json

# multiple-cpp
python main.py \
--model=bigcode/starcoder2-7b \
--peft_model=path_to_the_model/peft_50  \
--max_length_generation=1024  \
--batch_size=4 \
--use_auth_token \
--allow_code_execution \
--save_generations  \
--save_references \
--tasks=multiple-cpp \
--metric_output_path=./bigcode/starcoder2-7b/evaluation_results_multiple_cpp.json
```

### Results

__Average__: 44.08

__MBPP__: 48.6

__HumanEval__: 45.73

__MultiPL-E (JS)__: 38.5

__MultiPL-E (C++)__: 43.48


### Framework versions

- PEFT 0.14.0