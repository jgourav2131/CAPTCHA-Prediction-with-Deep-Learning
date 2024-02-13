# HexaCAPTCHA Solver Design Documentation

This document outlines the development and implementation strategy for a HexaCAPTCHA solver. The solver is designed to process 500 × 100 images, each containing a hexadecimal number, and return the parity (even or odd) of that number. The approach integrates computer vision techniques for preprocessing and a Convolutional Neural Network (CNN) for character recognition.

## Methodology

### Stage 1: Image Preprocessing

#### Stray Line Removal
- **Process**: The initial step involves dilating and then eroding the image to eliminate stray lines while preserving the characters.
- **Result**: Characters are restored to a condition similar to their original state without stray lines.

#### Morphological Transformations
- **Process**: To emphasize character margins, a morphological kernel is applied, making the margins bold.
- **Outcome**: The transformed image highlights the character margins for easier segmentation.

#### Character Segmentation
- **Process**: The largest connected component, typically enclosing the characters, is identified and extracted using OpenCV's connected components function with 8-connectivity.
- **Outcome**: Characters are segmented for individual recognition.

#### Contour Processing
- **Process**: Contours and their hierarchy are obtained to remove random white splotches. The algorithm focuses on the highest level contours and approximates each contour to a suitable shape. Contours with a significant area difference are discarded.
- **Outcome**: Only relevant contours, representing characters, are retained.

#### Image Division and Segmentation
- **Process**: The image is divided into four parts based on the x-coordinates of white pixels, leading to the identification of segments for each character.
- **Outcome**: Segments are ready for individual processing and analysis.

### Stage 2: Character Recognition using CNN

#### Dataset Preparation
- **Process**: Segments representing the last character of the hexadecimal number are labeled based on their parity and used to create a training dataset for the CNN.
- **Outcome**: A labeled dataset for training the CNN model.

#### Neural Network Architecture
- **Implementation**: TensorFlow is used to build the CNN model, consisting of multiple Conv2D and MaxPooling2D layers for feature extraction, followed by Dense layers for classification.
- **Hyperparameters and Training**: The model is trained using binary cross-entropy as the loss function. Hyperparameters were tuned through grid search to find the optimal settings for accuracy.

#### Model Performance
- **Size**: 7.5MB
- **Testing Time per Image**: 0.068 seconds
- **Test Loss**: 0.029
- **Test Accuracy**: 99.75%
- **Parity Score**: 1

## Conclusion

The HexaCAPTCHA solver demonstrates high accuracy in determining the parity of hexadecimal numbers in images. Through a combination of strategic image preprocessing and an efficiently trained CNN model, the system provides a reliable solution for CAPTCHA challenges. This methodology underscores the potential of integrating computer vision and deep learning techniques for solving complex pattern recognition tasks.

## Credits

The development of this solver was inspired by and adapted from various sources in the fields of computer vision and deep learning. Special thanks to the TensorFlow team and the OpenCV community for their comprehensive documentation and tutorials, which were instrumental in the implementation of this project.
