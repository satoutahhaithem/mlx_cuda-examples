# MLX Examples - Llama Inference with Custom CUDA Build

This branch contains an example of how to run Llama inference using a custom build of MLX with CUDA support.

## Running the Llama Inference

1.  **Create a virtual environment:**

    ```bash
    python3 -m venv .venv
    ```

2.  **Install the required packages using a proxy:**

    Replace the proxy URL with your own.

    ```bash
    export https_proxy=http://user:password@host:port && \
    export http_proxy=http://user:password@host:port && \
    export HTTPS_PROXY=http://user:password@host:port && \
    export HTTP_PROXY=http://user:password@host:port && \
    .venv/bin/pip install -r llms/llama/requirements.txt
    ```

3.  **Install your custom MLX build:**

    Replace `~/mlx` with the path to your custom MLX build.

    ```bash
    .venv/bin/pip install -e ~/mlx
    ```

4.  **Initialize the submodule to get the model:**

    ```bash
    git submodule update --init
    ```

5.  **Convert the model to MLX format:**

    ```bash
    .venv/bin/python llms/llama/convert.py --torch-path llms/llama/tiny_llama_torch --model-name tiny_llama
    ```

6.  **Run the Llama inference:**

    ```bash
    .venv/bin/python llms/llama/llama.py --model-path mlx_model --gpu
    ```

## Results

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
