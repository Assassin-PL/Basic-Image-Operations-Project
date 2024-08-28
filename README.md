# Basic Image Operations Project

This project focuses on performing basic image operations using Python, specifically utilizing the OpenCV and NumPy libraries. The project is divided into three groups of operations:

- **G1**: Sobel, Pyr Down, Blend, Pyr Up
- **G2**: Image derivatives (dx, dy), Harris corners
- **G3**: Kirsch, Canny

## Project Structure

The project is organized as follows:

- **klasy.py**: Contains the `Obrazek` class, which implements various image processing operations such as Sobel filtering, Pyr Down, Pyr Up, and Kirsch edge detection.
- **main.py**: The main script that demonstrates the use of the `Obrazek` class to apply different image operations, including converting images to grayscale, detecting edges using Sobel and Kirsch operators, and saving the processed images.
- **sprawdzenie.py**: A script that includes functions for generating Kirsch masks and applying them to an image to perform edge detection.

## Requirements

- Python 3.x
- NumPy
- OpenCV
- PIL (Python Imaging Library)
- Matplotlib

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/your-repo-name.git
2. Install the required dependencies:
   ```bash
   pip install numpy opencv-python pillow matplotlib
## Running the Project
1. Load and Process Images: Load the image using the Obrazek class and apply various image processing techniques.
   ```python
   obraz = Obrazek('pg.jpg', True)
2. Apply Sobel Filtering: Apply the Sobel filter to detect edges in the image.
     ```python
    obraz.wykryj_krawedzie_sobela()
3. Apply Kirsch Filtering: Detect edges using the Kirsch operator and save the result.
   ```python
   obraz.wykryj_krawedzie_kircha()
   obraz.zwroc_obraz('Kirsch.jpg')
4. Apply Canny Edge Detection: Detect edges using the Canny operator and save the result.
   ```python
   obraz.wykryj_krawedzie_canny()
   obraz.zwroc_obraz('Canny.jpg')
## Example Usage

1. The project includes an example script in main.py that demonstrates how to use the Obrazek class to perform the above operations. Simply run the script after setting the paths to your images.
   ```bash
   python main.py

## Image Operations Overview

### G1: Sobel, Pyr Down, Blend, Pyr Up

- **Sobel**: 
  - The Sobel operator is a discrete differentiation operator, computing an approximation of the gradient of the image intensity function. The operator uses two convolution kernels, one for the horizontal changes (dx) and one for the vertical changes (dy). These kernels are typically of size 3x3.
  - The formula for the Sobel operator is as follows:
    - **Horizontal Kernel (Sobel X)**:
      ```
      [-1  0  1]
      [-2  0  2]
      [-1  0  1]
      ```
    - **Vertical Kernel (Sobel Y)**:
      ```
      [-1 -2 -1]
      [ 0  0  0]
      [ 1  2  1]
      ```
  - The Sobel operator is applied separately to the X and Y directions, and the gradient magnitude is computed as:
    ```
    G = sqrt((Gx)^2 + (Gy)^2)
    ```
  - In this project, the Sobel operator can be applied using the `wykryj_krawedzie_sobela()` method, which processes the image to highlight the edges based on intensity changes.

- **Pyr Down**:
  - The Pyr Down operation reduces the size of the image by half. This is done by applying a Gaussian filter to the image to smooth it and then downsampling the image.
  - The operation reduces both the width and height of the image by a factor of 2, and it is commonly used to create an image pyramid, where each level is a reduced version of the previous one.
  - In OpenCV, this operation can be performed using `cv2.pyrDown()`, which applies the Gaussian filter and downsamples the image.

- **Blend**:
  - Image blending is the process of combining two images into one by specifying the weight (or opacity) of each image. The formula for linear blending is:
    ```
    Blended Image = alpha * Image1 + (1 - alpha) * Image2
    ```
  - Here, `alpha` is a scalar value between 0 and 1 that controls the contribution of each image to the final result. 
  - Blending is commonly used in image compositing where you might want to combine different layers of images to create a final output.
  - This operation can be performed using `cv2.addWeighted()` in OpenCV, which allows specifying the weights for each image.

- **Pyr Up**:
  - The Pyr Up operation increases the size of the image by a factor of 2. It essentially upscales the image by inserting interpolated pixels between the original pixels to increase the resolution.
  - This operation can be seen as the reverse of the Pyr Down operation and is typically used to reconstruct an image at a higher resolution from its pyramid representation.
  - In OpenCV, this can be done using `cv2.pyrUp()`, which upsamples the image by a factor of 2 in both dimensions.

### G2: Image Derivatives (dx, dy), Harris Corners

- **Image Derivatives**:
  - Image derivatives represent the rate of change of pixel intensity values. The first derivative in the x-direction (dx) and y-direction (dy) are essential for edge detection, as they highlight regions with significant intensity changes.
  - The derivatives can be computed using convolution with derivative kernels, such as:
    - **Derivative Kernel for dx**:
      ```
      [-1  0  1]
      ```
    - **Derivative Kernel for dy**:
      ```
      [-1]
      [ 0]
      [ 1]
      ```
  - These derivatives are combined to form the gradient magnitude:
    ```
    Gradient Magnitude = sqrt((dx)^2 + (dy)^2)
    ```
  - In this project, the derivatives are used to enhance edge detection in operations like Sobel and Canny.

- **Harris Corners**:
  - The Harris Corner Detector is an algorithm to detect points in an image where the intensity changes significantly in all directions, which is indicative of a corner. It computes the matrix:
    ```
    M = [ Ix^2  Ix*Iy ]
        [ Ix*Iy  Iy^2 ]
    ```
    where `Ix` and `Iy` are the derivatives in the x and y directions, respectively.
  - The response of the detector is computed as:
    ```
    R = det(M) - k * (trace(M))^2
    ```
    where `det(M)` is the determinant of the matrix and `trace(M)` is the trace. `k` is a constant typically between 0.04 and 0.06.
  - Corners are identified by thresholding the response `R`, where strong corners correspond to large values of `R`.
  - In OpenCV, Harris corners can be detected using `cv2.cornerHarris()` which computes the response for each pixel and identifies the corners based on a threshold.

### G3: Kirsch, Canny

- **Kirsch**:
  - The Kirsch operator is used for edge detection and is based on a set of eight convolution kernels that detect edges in eight different compass directions (North, South, East, West, and the diagonals).
  - The eight convolution kernels are:
    ```
    North:         Northeast:       East:         Southeast:
    [-3 -3  5]     [-3  5  5]       [ 5  5  5]    [ 5  5 -3]
    [-3  0  5]     [-3  0  5]       [-3  0  5]    [-3  0 -3]
    [-3 -3  5]     [-3 -3 -3]       [-3 -3 -3]    [ 5 -3 -3]
    
    South:         Southwest:       West:         Northwest:
    [ 5 -3 -3]     [ 5  5 -3]       [-3 -3 -3]    [-3 -3 -3]
    [ 5  0 -3]     [-3  0 -3]       [ 5  0 -3]    [-3  0  5]
    [ 5 -3 -3]     [-3 -3 -3]       [ 5  5  5]    [ 5  5  5]
    ```
  - The Kirsch operator applies each kernel to the image, and the maximum absolute value of the convolution result across all directions is taken as the final edge strength at each pixel.
  - In this project, the Kirsch operator can be applied using the `wykryj_krawedzie_kircha()` method, which processes the image to highlight the edges detected by the Kirsch operator.

- **Canny**:
  - The Canny edge detector is a multi-step algorithm used to detect edges in an image. It involves the following steps:
    1. **Noise Reduction**: The image is smoothed using a Gaussian filter to reduce noise.
    2. **Gradient Calculation**: The gradient magnitude and direction are computed at each pixel using Sobel filters.
    3. **Non-Maximum Suppression**: Spurious responses to edge detection caused by noise and color variation are suppressed by thinning out the edges.
    4. **Double Thresholding**: Edge pixels are classified as strong, weak, or non-edges based on two threshold values.
    5. **Edge Tracking by Hysteresis**: Weak edges that are connected to strong edges are kept, while others are discarded.
  - The result is a binary image where the edges are marked with white pixels, and the background is black.
  - In OpenCV, Canny edge detection can be performed using `cv2.Canny()`, where you can specify the lower and upper thresholds for edge detection.
  - In this project, the Canny edge detector can be applied using the `wykryj_krawedzie_canny()` method.


