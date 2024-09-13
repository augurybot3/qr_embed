# QR Embed

This project implements a technique to embed and extract QR codes into images using Discrete Cosine Transform (DCT) coefficients. This method is based on applying DCT to the image's Y channel (luminance) and embedding a tiled QR code across the entire image. The QR code is then extracted by analyzing differences in the Y channel between the original and watermarked images.

## Table of Contents
- [Requirements](#requirements)
- [Setup](#setup)
- [Usage](#usage)
- [Functions](#functions)
- [Notes](#notes)

## Requirements

To run this project, you need the following libraries:

- OpenCV (`cv2`)
- NumPy
- qrcode
- PIL (Pillow)
- pyzbar
- Matplotlib

You can install the required packages using pip:

```bash
pip install opencv-python numpy qrcode[pil] pillow pyzbar matplotlib
```

## Setup

1. **Provide image path**: The image must be at least 512x512 in size.
    - Example: `image = '/path/to/your/image.jpeg'`
   
2. **Provide QR code data**: You can input a `str` or `dict`.
    - Example: `qr_code_data = 'https://example.com'`
   
3. **Provide path for the embedded image**: The output will be a watermarked image with the QR code embedded in it.
    - Example: `output_filename = 'watermarked_image.jpg'`

## Usage

1. **Load and preprocess the image**:
   - Crop or pad the image dimensions to ensure they are divisible by 8 for DCT processing.
   
2. **Generate tiled QR code**:
   - A QR code is generated based on the provided data and tiled to cover the image's full dimensions.

3. **Apply DCT**:
   - The DCT is applied to the image’s Y channel (luminance).

4. **Embed QR code into DCT coefficients**:
   - The QR code is embedded into the high-frequency DCT coefficients.

5. **Inverse DCT**:
   - Convert the image back into the spatial domain by applying the inverse DCT.

6. **Extract and decode the QR code**:
   - Compare the Y channel differences between the original and watermarked images to extract and verify the QR code.

## Functions

### 1. `crop_or_pad_image_to_divisible_by_8(image)`
Ensures that the image dimensions are divisible by 8, which is necessary for block-wise DCT operations.

### 2. `generate_tiled_qr_code(data, image_width, image_height, qr_code_size=512)`
Generates a QR code and tiles it across the entire image area.

### 3. `apply_dct(image)`
Applies DCT to the image’s Y channel (luminance).

### 4. `embed_tiled_qr_in_dct(dct_blocks, tiled_qr_image, embed_strength=25.0)`
Embeds the tiled QR code into the high-frequency DCT coefficients of the image.

### 5. `inverse_dct_and_save(dct_blocks, ycrcb, output_filename)`
Applies the inverse DCT and saves the watermarked image.

### 6. `compare_y_channels(original_image, watermarked_image)`
Compares the Y channels of the original and watermarked images to calculate the difference.

### 7. `extract_qr_from_dct_with_diff(dct_blocks, y_diff)`
Extracts the QR code based on the differences in DCT coefficients between the original and watermarked images.

### 8. `verify_qr_tiles(extracted_qr_image, qr_code_size=256)`
Decodes the QR code from the extracted image and verifies its data.

## Notes

- The input image must be at least **512 x 512** pixels. This requirement is due to the lossiness of the process when embedding the QR code into the DCT coefficients. You can change the `qr_code_size` parameter in the `generate_tiled_qr_code()` function if needed.
  
- Ensure that the image has appropriate contrast and size for the QR code to be properly embedded and extracted.

---

This README provides the steps and explanation for embedding and extracting QR codes using DCT coefficients in your Jupyter project.
