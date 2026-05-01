# Fish-Species-Classification-CNN

# Dataset Link
https://www.kaggle.com/datasets/markdaniellampa/fish-dataset/data

---

## Steps to run locally
```
Windows → WSL2 (Ubuntu) → CUDA 12.x → TensorFlow (latest) → RTX 3060 ✓
```

---

## Step 1: Enable WSL2
In PowerShell **as Administrator**:
```powershell
wsl --install
# Restart your PC when prompted
```
After restart, Ubuntu will finish installing. Set a username and password when asked.

---

## Step 2: Don't install CUDA inside WSL2
Your Windows NVIDIA driver (576.40) **automatically exposes the GPU to WSL2**. Just verify it works:
```bash
nvidia-smi  # Run this inside Ubuntu terminal — should show your RTX 3060
```

---

## Step 3: Set up Python environment in WSL2
```bash
# Update packages
sudo apt update && sudo apt upgrade -y

# Install pip and venv
sudo apt install python3-pip python3-venv -y

# Create virtual environment for your project
python3 -m venv ~/cnn_env
source ~/cnn_env/bin/activate
```

---

## Step 4: Install TensorFlow
```bash
pip install tensorflow[and-cuda]
```
> This single command installs TensorFlow **with** all the right CUDA and cuDNN libraries automatically — no manual CUDA setup needed inside WSL2.

---

## Step 5: Verify GPU is detected
```python
import tensorflow as tf
print(tf.config.list_physical_devices('GPU'))
# Expected: [PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
print(tf.test.gpu_device_name())
# Expected: /device:GPU:0
```

---

## Step 6: Access your project files from WSL2
Your `C:\GitHub\Projects\Fish-Species-Classification-CNN` folder is accessible inside WSL2 at:
```bash
cd /mnt/c/GitHub/Projects/Fish-Species-Classification-CNN
```

---

## Step 7: Run Jupyter from WSL2
```bash
pip install jupyter
jupyter notebook --no-browser --port=8888
```
Then open the URL it gives you (starting with `http://127.0.0.1:8888/...`) in your **Windows browser** — it works seamlessly.

---

## Quick optimizations for your 6GB VRAM

Since the RTX 3060 Laptop has 6GB, add this at the top of your notebook to prevent TensorFlow from grabbing all VRAM at once:

```python
import tensorflow as tf

# Allow memory growth (allocates only what's needed)
gpus = tf.config.list_physical_devices('GPU')
if gpus:
    tf.config.experimental.set_memory_growth(gpus[0], True)

# Confirm GPU
print("GPU available:", len(gpus) > 0)
```

And enable mixed precision for faster training:
```python
from tensorflow.keras import mixed_precision
mixed_precision.set_global_policy('mixed_float16')
```

---
