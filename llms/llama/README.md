# Llama

An example of generating text with Llama (1 or 2) using MLX.

Llama is a set of open source language models from Meta AI Research[^1][^2]
ranging from 7B to 70B parameters. This example also supports Meta's Llama Chat
and Code Llama models, as well as the 1.1B TinyLlama models from SUTD.[^3]

### Setup

Install the dependencies:

```
pip install -r requirements.txt
```

Next, download and convert the model. If you do not have access to the model
weights you will need to request access from Meta:

- [Request Llama v1](https://docs.google.com/forms/d/e/1FAIpQLSfqNECQnMkycAp2jP4Z9TFX0cGR4uf7b_fBxjY_OjhJILlKGA/viewform)
- [Request Llama v2](https://ai.meta.com/resources/models-and-libraries/llama-downloads/)

> [!TIP] Alternatively, you can also download a few converted checkpoints from
> the [MLX Community](https://huggingface.co/mlx-community) organization on
> Hugging Face and skip the conversion step.

You can download the TinyLlama models directly from [Hugging
Face](https://huggingface.co/TinyLlama).

Convert the weights with:

```
python convert.py --torch-path <path_to_torch_model>
```

To generate a 4-bit quantized model use the `-q` flag:

```
python convert.py --torch-path <path_to_torch_model> -q
```

For TinyLlama use

```
python convert.py --torch-path <path_to_torch_model> --model-name tiny_llama
```

By default, the conversion script will make the directory `mlx_model` and save
the converted `weights.npz`, `tokenizer.model`, and `config.json` there.


### Run

Once you've converted the weights to MLX format, you can interact with the
LlamA model:

```
python llama.py --prompt "hello"
```

Run `python llama.py --help` for more details.

[^1]: For Llama v1 refer to the [arXiv paper](https://arxiv.org/abs/2302.13971) and [blog post](https://ai.meta.com/blog/large-language-model-llama-meta-ai/) for more details.
[^2]: For Llama v2 refer to the [blob post](https://ai.meta.com/llama/)
[^3]: For TinyLlama refer to the [gihub repository](https://github.com/jzhang38/TinyLlama?tab=readme-ov-file)


### Running with a Custom MLX Build and Proxy

If you are using a custom build of MLX and are behind a proxy, follow these steps:

1.  **Create a virtual environment:**

    ```bash
    python3 -m venv .venv
    ```

2.  **Install the required packages using a proxy:**

    Replace the proxy URL with your own.

    ```bash
    export https_proxy=http://user:password@host:port &amp;&amp; \
    export http_proxy=http://user:password@host:port &amp;&amp; \
    export HTTPS_PROXY=http://user:password@host:port &amp;&amp; \
    export HTTP_PROXY=http://user:password@host:port &amp;&amp; \
    .venv/bin/pip install -r requirements.txt
    ```

3.  **Install your custom MLX build:**

    Replace `~/mlx` with the path to your custom MLX build.

    ```bash
    .venv/bin/pip install -e ~/mlx
    ```

4.  **Download the TinyLlama model:**

    ```bash
    git lfs install &amp;&amp; \
    git clone https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0 tiny_llama_torch
    ```

5.  **Convert the model to MLX format:**

    ```bash
    .venv/bin/python convert.py --torch-path tiny_llama_torch --model-name tiny_llama
    ```

6.  **Run the Llama inference:**

    ```bash
    .venv/bin/python llama.py --model-path mlx_model --gpu
    ```

### Results

The model generates text, but the output is not very coherent. This is expected from a small model like TinyLlama.

```
Press enter to start generation
------
In the beginning the Universe was created.


Verse 2:
The Universe was created with a great big bang.

Chorus:
The Universe is a vast and endless space.

Verse 3:
The Universe is full of wonders and mysteries.

Chorus:
The Universe is a vast and endless space.

Bridge:
The Universe is a vast and endless space.

Chorus:
The Universe
------
[INFO] Prompt processing: 12.523 s
[INFO] Full generation: 361.172 s
```
