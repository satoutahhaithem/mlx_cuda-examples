# Running Llama with MLX and CUDA on Linux

This guide provides step-by-step instructions to run a Llama language model using MLX with NVIDIA GPU acceleration on a Linux system.

## Prerequisites

*   A Linux system with a compatible NVIDIA GPU.
*   NVIDIA drivers and the CUDA Toolkit installed. You can verify this by running `nvidia-smi`.

## 1. Clone the Repository

First, clone the `mlx-examples` repository from GitHub:

```bash
git clone https://github.com/ml-explore/mlx-examples.git
cd mlx-examples
```

## 2. Create and Activate a Virtual Environment

It is highly recommended to use a Python virtual environment to avoid conflicts with other projects.

```bash
python3 -m venv .venv
source .venv/bin/activate
```

## 3. Install Dependencies

Install the necessary Python packages.

**(Optional) If you are behind a proxy, export the following variables first:**

```bash
export https_proxy="http://user:password@host:port"
export http_proxy="http://user:password@host:port"
export HTTPS_PROXY="http://user:password@host:port"
export HTTP_PROXY="http://user:password@host:port"
```

Now, install `mlx`, `mlx-cuda`, and the other requirements:

```bash
pip install mlx mlx-cuda
pip install -r llms/llama/requirements.txt
```

## 4. Download and Convert Model Weights

You need to download the Llama model weights and convert them to the MLX format.

**Download the model:**

You can download the official Llama models from Meta, or use a compatible model from Hugging Face (e.g., TinyLlama). For this example, we'll use TinyLlama.

```bash
git lfs install
git clone https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0 tiny_llama_torch
```

**Convert the weights:**

```bash
python llms/llama/convert.py --torch-path tiny_llama_torch --model-name tiny_llama
```

This will create a `mlx_model` directory containing the converted weights.

## 5. Run the Llama Model on the GPU

You are now ready to run the model. Use the `--gpu` flag to enable GPU acceleration.

```bash
python llms/llama/llama.py --model-path mlx_model --gpu --prompt "hello"
```

## Verification

While the script is running, you can open another terminal and run `nvidia-smi` to monitor GPU utilization. You should see a process running on one of your GPUs.

## Performance

The performance of the model can vary depending on your hardware. Here is an example of the output you might see:

```
[INFO] Prompt processing: 6.111 s
[INFO] Full generation: 7.541 s
```

While the script is running, you can open another terminal and run `nvidia-smi` to monitor GPU utilization. You should see a process running on one of your GPUs, which confirms that the model is leveraging the GPU for acceleration.

## Troubleshooting

### GPU Not Being Utilized

If you run the script with the `--gpu` flag but `nvidia-smi` shows no GPU utilization, you may have conflicting packages installed. For more details, see [this GitHub issue](https://github.com/ml-explore/mlx-examples/issues/1376).

**Cause:**

This issue can be caused by:
1.  **Conflicting `mlx` Installations:** A local or development version of `mlx` might be installed alongside `mlx-cuda`, preventing the GPU backend from being used.
2.  **Dependency Conflicts:** Other packages (like `torch`) might have dependencies that conflict with the NVIDIA libraries required by `mlx-cuda`.

**Solution:**

The most reliable solution is to use a clean virtual environment and perform a clean installation of the required packages.

1.  **Ensure you are in your virtual environment.**
2.  **Uninstall any existing `mlx` installations:**
    ```bash
    pip uninstall -y mlx mlx-cuda
    ```
3.  **Reinstall `mlx` and `mlx-cuda`:**
    ```bash
    pip install mlx-cuda
    pip install mlx
    ```
This ensures that the correct versions of the libraries are installed without conflicts.