Ayna ML Assignment - UNet-Based Polygon Colorizer  
Submitted by: Challuri Ashrith  
Role: ML Intern Candidate  

---

Problem Statement

This project focuses on training a UNet model to generate a "colored polygon image" based on:

- An input image of a polygon (shape only)
- A text input (color name, e.g., "blue", "yellow")

The model learns to fill the polygon with the correct color based on both inputs.

---

Hyperparameters

| Hyperparameter       | Value / Choice        | Reason                                                                 |
|----------------------|-----------------------|------------------------------------------------------------------------|
| Learning Rate        | `0.001`               | A good default for Adam; worked well during training                   |
| Optimizer            | `Adam`                | Efficient for training deep networks and fast convergence              |
| Loss Function        | `MSELoss`             | Suitable for pixel-wise image generation tasks                         |
| Batch Size           | `8`                   | Balanced between speed and memory constraints (Google Colab)          |
| Epochs               | `20`                  | Enough to reach convergence and stable predictions                     |
| Image Size           | `128 x 128`           | Standard size for fast processing while retaining shape details        |

---

Architecture

- Base Model: UNet (custom implementation)
- Input Channels:  
  - 1 for grayscale polygon image  
  - `n` channels for one-hot encoded color name (conditioned input)
- Output Channels: 3 (RGB)
- Design Notes:
  - Added the color vector as a second channel, expanded spatially and concatenated with the polygon image.
  - Used 3 encoder blocks, a bottleneck, and 3 decoder blocks with skip connections.
- No additional ablations were done as the base model worked well for this task.

---

Training Dynamics

- Training was stable over 20 epochs.
- The loss consistently decreased and showed no overfitting on the validation set.
- Used Weights & Biases (wandb) for logging:
  - Live monitoring of loss
  - Run comparisons
- Qualitative Results:
  - Output colors were close to the expected ones.
  - Shapes were preserved well.

---

Failure Cases & Fixes

| Issue                            | Observation / Fix                                              |
|----------------------------------|----------------------------------------------------------------|
| Color bleeding outside shapes    | Reduced after using consistent image resizing (128x128)        |
| Model confused similar colors    | Improved slightly after training for more epochs               |
| Model mismatch during inference  | Fixed by saving and reusing `color_names.pkl` and matching channels |

---

Key Learnings

- How to implement a custom UNet from scratch using PyTorch
- How to condition a model using text-based input (color vector)
- Hands-on practice with custom datasets, transforms, and dataloaders
- Used Weights & Biases for tracking experiments effectively
- Learned about GPU/CPU device handling and avoiding model loading errors

---

Files Submitted

- `training.ipynb`: Training notebook with wandb tracking  
- `inference.ipynb`: Demonstration of predictions with visuals  
- `unet_colorizer.pth`: Trained model weights  
- `color_names.pkl`: Required to run the model  
- This `README.md` file  
