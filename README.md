# Workshop 4: Coin Detection using OpenCV
# Name: Sowmiya R
# Reg No: 212225040420

## AIM

To detect and count coins in an image using Python and OpenCV by applying image preprocessing, edge detection, contour detection, and circular boundary identification.

## SOFTWARE REQUIRED

- Python
- OpenCV
- NumPy
- Matplotlib
- Jupyter Notebook
- Anaconda (optional)

## INSTALLATION

Install the required libraries using the following command:

```bash
pip install opencv-contrib-python-headless numpy matplotlib
```

## ALGORITHM

### Step 1: Import Libraries
- Import OpenCV for image processing.
- Import NumPy for numerical operations.
- Import Matplotlib for displaying images.

### Step 2: Read the Input Image
- Load the coin image using `cv2.imread()`.
- Check whether the image is loaded successfully.

### Step 3: Convert to Grayscale
- Convert the original image into grayscale using `cv2.cvtColor()`.
- Simplify the image for further processing.

### Step 4: Apply Gaussian Blur
- Apply Gaussian blur using `cv2.GaussianBlur()`.
- Reduce noise and unnecessary details in the image.

### Step 5: Perform Edge Detection
- Apply Canny edge detection using `cv2.Canny()`.
- Identify the boundaries of the coin.

### Step 6: Detect Contours
- Find contours using `cv2.findContours()`.
- Identify the circular boundaries in the image.
- Filter unwanted contours based on their area.

### Step 7: Detect and Count Coins
- Calculate the minimum enclosing circle for each detected contour.
- Draw circular boundaries around the detected coins.
- Mark the center of each coin.
- Count the detected coins.

### Step 8: Display the Results
- Display the original image.
- Display the detected coin image.
- Display the intermediate processing images.
- Print the total number of detected coins.

## PROGRAM

```
import cv2
import numpy as np
import matplotlib.pyplot as plt
import os

image_path = "coin.jpg"

# Load image
image = cv2.imread(image_path)
if image is None:
    raise FileNotFoundError(f"Image not found at {image_path}")

gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)


blurred = cv2.GaussianBlur(gray, (11, 11), 0)
edges = cv2.Canny(blurred, 30, 150)

contours, _ = cv2.findContours(edges.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

output = image.copy()

coin_count = 0
for contour in contours:
    # Filter out small contours (noise)
    area = cv2.contourArea(contour)
    if area < 300:
        continue

    # Draw contour
    ((x, y), radius) = cv2.minEnclosingCircle(contour)
    if radius > 15:
        coin_count += 1
        cv2.circle(output, (int(x), int(y)), int(radius), (0, 255, 0), 2)
        cv2.putText(output, f"{coin_count}", (int(x - 10), int(y + 10)),
                    cv2.FONT_HERSHEY_SIMPLEX, 0.6, (0, 0, 255), 2)

print(f"Total coins detected: {coin_count}")


plt.figure(figsize=(12, 6))
plt.subplot(1, 2, 1)
plt.title("Original Image")
plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
plt.axis("off")
plt.subplot(1, 2, 2)
plt.title(f"Detected Coins: {coin_count}")
plt.imshow(cv2.cvtColor(output, cv2.COLOR_BGR2RGB))
plt.axis("off")
plt.show()

fig, axs = plt.subplots(1, 5, figsize=(20, 6))

axs[0].imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
axs[0].set_title("Original Image")
axs[0].axis("off")

axs[1].imshow(gray, cmap='gray')
axs[1].set_title("Grayscale Image")
axs[1].axis("off")

axs[2].imshow(blurred, cmap='gray')
axs[2].set_title("Blurred Image")
axs[2].axis("off")

axs[3].imshow(edges, cmap='gray')
axs[3].set_title("Edge Detection (Canny)")
axs[3].axis("off")

axs[4].imshow(cv2.cvtColor(output, cv2.COLOR_BGR2RGB))
axs[4].set_title(f"Detected Coins: {coin_count}")
axs[4].axis("off")

plt.tight_layout()
plt.show()
```


## OUTPUT

The following outputs are obtained:

### 1. Original Image and Detected Coin Image
<img width="697" height="342" alt="image" src="https://github.com/user-attachments/assets/6c7076ab-4812-4b60-9904-0cf3ccc20db6" />

### 2. Image Processing Results
<img width="958" height="222" alt="image" src="https://github.com/user-attachments/assets/d68357e2-dcf7-4655-a2ef-17760ff120f8" />


## RESULT

Thus, coin detection and counting were successfully performed using Python and OpenCV. Image preprocessing, Gaussian blurring, edge detection, and contour analysis were used to identify circular coin boundaries and count the detected coins.
