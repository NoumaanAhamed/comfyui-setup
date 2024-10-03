# ComfyUI Setup Guide

This guide will help you set up ComfyUI locally using Python, PyTorch, and Hugging Face.

## Prerequisites

- Python 3.x
- Git
- NVIDIA GPU with CUDA 12.1 or compatible
- A Hugging Face account

### Step 0: Get a GPU Provider (e.g., AWS SageMaker Studio Labs)

If you don't have access to a GPU locally, you can use a cloud provider such as AWS SageMaker Studio Labs to get a free GPU environment. Here's how:

1. Go to [AWS SageMaker Studio Labs](https://studiolab.sagemaker.aws/).
2. Sign up for an account or log in if you already have one.
3. Once logged in, create a new project.
4. Choose an environment that supports GPU (NVIDIA CUDA compatible).
5. Start the instance and connect to the Jupyter environment.
6. Use the terminal within Jupyter to follow the rest of the steps below.

For other GPU providers refer to this [blog](https://noumaan.bearblog.dev/alternatives-to-googlecolab/)

Once you have access to a GPU environment, proceed with the setup steps.

### Step 1: Clone the ComfyUI Repository

First, create a directory for your project and clone the ComfyUI repository.

```bash
# Create and navigate to the project directory
mkdir comfyui-testing
cd comfyui-testing/

# Clone the ComfyUI repository from GitHub
git clone https://github.com/comfyanonymous/ComfyUI.git
```

### Step 2: Set Up a Python Virtual Environment

Create and activate a virtual environment to isolate the Python dependencies.

```bash
# Create a virtual environment
python -m venv .venv

# Activate the virtual environment
source ./.venv/bin/activate
```

### Step 3: Verify GPU Setup (Optional)

If you have an NVIDIA GPU, you can check its status with the following command:

```bash
nvidia-smi
```

### Step 4: Install PyTorch with CUDA

Install PyTorch, TorchVision, and Torchaudio with CUDA support (12.1 version) for GPU acceleration.

```bash
pip install torch torchvision torchaudio
```

### Step 5: Install Project Dependencies

Navigate to the `ComfyUI` directory and install the required dependencies from the `requirements.txt` file.

```bash
cd ComfyUI/
pip install -r requirements.txt
```

### Step 6: Run ComfyUI

Now that the dependencies are installed, you can start the ComfyUI application.

```bash
# Run the application
python main.py
```

### Step 7: Optional: Setup Ngrok for External Access

To make your local server accessible externally, you can use Ngrok. Install it and set it up as follows:

```bash
pip install ngrok
```

```py
# import ngrok python sdk
import ngrok
import time

# Establish connectivity
listener = ngrok.forward(8188, authtoken_from_env=True)

# Output ngrok url to console
print(f"Ingress established at {listener.url()}")

# Keep the listener alive
try:
    while True:
        time.sleep(1)
except KeyboardInterrupt:
        print("Closing listener")
```

```bash
# Run ngrok with your auth token from https://dashboard.ngrok.com/
NGROK_AUTHTOKEN=<YOUR_NGROK_TOKEN> python ngrok-python.py
```

### Step 8: Setup Hugging Face CLI

Install the Hugging Face CLI tool to download models and log in to your account using https://huggingface.co/settings/tokens

```bash
pip install -U "huggingface_hub[cli]"

# Log in to Hugging Face
huggingface-cli login

# Verify login
huggingface-cli whoami
```

### Step 9: Using ComfyUI

1. Download a checkpoint file Place the file under ComfyUI/models/checkpoints.

```bash
huggingface-cli download Comfy-Org/stable-diffusion-v1-5-archive v1-5-pruned-emaonly.safetensors --local-dir .
```

3. Refresh the ComfyUI.

4. Click Load Default button to use the default workflow.

5. In the Load Checkpoint node, select the checkpoint file you just downloaded.

6. Click Queue Prompt and watch your image generated. Play around with the prompts to generate different images.

## Referred Docs/Links

- [AWS SageMaker Studio Labs](https://studiolab.sagemaker.aws/)
- [PyTorch Installation Guide](https://pytorch.org/get-started/locally/)
- [ComfyUI Docs](https://docs.comfy.org/get_started/introduction)
- [Hugging Face CLI Documentation](https://huggingface.co/docs/huggingface_hub/quick-start)
- [ngrok Docs](https://pypi.org/project/ngrok/)




### Conclusion

You have now successfully set up ComfyUI locally. You can access the interface via your local server, or use Ngrok for external access.
