# Neural Network Watermarking Against Trigger-Set Leakage and Extraction

This repository contains an experimental implementation of neural network watermarking with trigger-set protection. It supports watermark training, model attacks, watermark evaluation, trigger-set encryption, and trigger-set decryption.

## Features

- Watermark training from a clean model or from scratch.
- Clean-model training and standard classification evaluation.
- Model extraction, fine-tuning, pruning, quantization, and cross-architecture attacks.
- Traditional trigger-set verification and watermark/任务 coupling analysis.
- AES-based encryption and decryption of trigger images, labels, and serialized trigger objects.

## Repository layout

```text
main.py       Command-line entry point
models/       Model architectures and watermark key implementation
datasets/     Dataset loaders and preprocessing
train/        Training, evaluation, attack, and cryptographic utilities
config/       YAML experiment configurations
scripts/      Example experiment scripts
```

## Installation

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\\Scripts\\activate
pip install torch torchvision numpy pillow pyyaml matplotlib seaborn pandas cryptography tqdm opencv-python
```

Use a PyTorch build compatible with the CUDA version on the target machine.

## Basic usage

Train a clean model:

```bash
python main.py --config config/cifar10/cifar10_clean.yaml --runname cifar10_clean
```

Train a watermarked model:

```bash
python main.py --config config/cifar10/cifar10_watermark.yaml --runname cifar10_watermark --save_trigger
```

Evaluate a checkpoint:

```bash
python main.py --action evaluate --dataset cifar10 --arch resnet18 --checkpoint_path path/to/last.pt
```

Evaluate a trigger set explicitly:

```bash
python main.py --action eval_trigger --dataset cifar10 --arch resnet18 --model_path path/to/last.pt --trigger_path path/to/key.pt
```

Analyze the coupling between the watermark and the main task:

```bash
python main.py --action coupling_eval --dataset cifar10 --arch resnet18 --checkpoint_path path/to/last.pt
```

## Trigger-set encryption

```bash
python main.py --config config/cifar10/encrypt_trigger.yaml
python main.py --config config/cifar10/decrypt_trigger.yaml
```

Keep passwords, private trigger sets, model checkpoints, logs, and datasets out of a public repository.

## Configuration

Experiment parameters are stored in `config/`. Update local paths before running an experiment. Important options include `dataset`, `arch`, `epochs`, `batch_size`, `lr`, `key_num`, `key_target`, `clean_model_path`, `checkpoint_path`, and the trigger-encryption paths.

## Reproducibility

Set `--seed` where supported, preserve the YAML configuration for each run, and keep the generated logs, `conf.json`, and checkpoints. Results may vary with PyTorch, CUDA, GPU, and torchvision versions.

## Security and license

This code is intended for research. Encryption does not replace secure key management; keep encryption keys outside the repository. No license file is currently included. Add an appropriate license before public distribution.
