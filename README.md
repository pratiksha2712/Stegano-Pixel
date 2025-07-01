# Steganography Tool

> **Hide in Plain Sight** - A modern web application for concealing text messages and images within digital pictures using advanced steganographic techniques.

---

## What is Steganography?

Steganography is the practice of concealing information within other non-secret data or physical media. Unlike encryption, which scrambles data, steganography hides the very existence of the secret information, making it appear as if no hidden content exists.

## Project Capabilities

### **Text Concealment Module**
Hide secret messages within image pixels using LSB (Least Significant Bit) manipulation
- **Input**: Any text message + Cover image
- **Output**: Visually identical image containing hidden message
- **Capacity**: Depends on image size (approximately 1 character per 3 pixels)

### **Image Fusion Module** 
Combine two separate images into a single encrypted composite
- **Input**: Two images of identical dimensions
- **Output**: Single image that appears as noise but contains both originals
- **Recovery**: Perfect reconstruction of both original images

---

## Technology Implementation

**Core Framework**: Streamlit (Web Interface)
**Image Processing**: OpenCV + PIL/Pillow
**Mathematical Operations**: NumPy
**Development Environment**: Python 3.8+

### Dependencies
```
streamlit >= 1.28.0
opencv-python >= 4.8.0
Pillow >= 10.0.0
numpy >= 1.24.0
jupytext >= 1.14.0
```

---

## Quick Launch

### Setup Environment
```bash
# Clone project
git clone <repository-url>
cd steganography-tool

# Install requirements
pip install streamlit opencv-python pillow numpy jupytext

# Convert notebook (if needed)
jupytext --to .py Steganography.ipynb
```

### Start Application
```bash
streamlit run Steganography.py --server.enableXsrfProtection false
```

**Access**: Open browser to `http://localhost:8501`

---

## User Interface Guide

### **Tab 1: Text Operations**

**Encoding Workflow:**
1. Upload cover image (JPG/PNG/JPEG)
2. Enter secret message in text field
3. Specify output filename with extension
4. Click "Submit" → Message embedded invisibly

**Decoding Workflow:**
1. Upload steganographic image
2. Click "Decode" → Hidden message revealed

### **Tab 2: Image Operations**

**Encryption Workflow:**
1. Upload first image (cover)
2. Upload second image (hidden) - must match dimensions
3. Enter encrypted filename
4. Click "Encrypt" → Images combined

**Decryption Workflow:**
1. Upload encrypted composite image
2. Specify two output filenames
3. Click "Decrypt" → Original images extracted

---

## Technical Deep Dive

### **LSB Text Steganography Algorithm**

The application manipulates the least significant bits of pixel color values:

```
Original RGB: (11010110, 10110011, 01110101)
Message bit:  1
Modified:     (11010111, 10110011, 01110101)
               ↑
          LSB changed to match message bit
```

**Process Flow:**
1. Convert text → 8-bit binary representation
2. Sequentially modify LSBs of R,G,B values
3. Use 9th pixel LSB as termination indicator
4. Save modified image with imperceptible changes

### **4-Bit Image Fusion Algorithm**

Combines images by merging their most significant bits:

```
Image A pixel: 11010110 → Take 4 MSBs: 1101
Image B pixel: 10110001 → Take 4 MSBs: 1011
Result pixel:  11011011 (A_MSB + B_MSB)
```

**Advantages:**
- Preserves primary visual information from both images
- Allows perfect reconstruction with proper decoding
- Creates noise-like appearance for security

---

## Performance Characteristics

| Operation | Time Complexity | Space Complexity | Image Quality |
|-----------|----------------|------------------|---------------|
| Text Encode | O(n×m) | O(1) | Lossless |
| Text Decode | O(n×m) | O(k) | N/A |
| Image Encrypt | O(n×m×3) | O(n×m×3) | Degraded |
| Image Decrypt | O(n×m×3) | O(2×n×m×3) | Reconstructed |

*where n×m = image dimensions, k = message length*

---

## File Format Support Matrix

| Format | Text Encode | Text Decode | Image Encrypt | Image Decrypt |
|--------|-------------|-------------|---------------|---------------|
| **JPG** | ✅ | ✅ | ✅ | ✅ |
| **PNG** | ✅ | ✅ | ✅ | ✅ |
| **JPEG** | ✅ | ✅ | ✅ | ✅ |
| **TIFF** | ❌ | ❌ | ✅ | ✅ |

---

## Security Analysis

### **Strengths**
- **Visual Stealth**: Human eye cannot detect modifications
- **Format Preservation**: Maintains original file characteristics
- **Scalability**: Works with various image sizes

### **Vulnerabilities**
- **Statistical Analysis**: Vulnerable to chi-square attacks
- **Compression**: JPEG compression may corrupt hidden data
- **Pattern Detection**: Regular LSB patterns detectable by algorithms

### **Recommended Use Cases**
- Personal data backup within images
- Educational demonstrations of steganography
- Proof-of-concept for digital watermarking

---

## Code Architecture

```
steganography-tool/
│
├── Steganography.py           # Main application
├── Steganography.ipynb        # Development notebook
├── requirements.txt           # Dependencies
│
├── Core Functions:
│   ├── genData()                 # Text to binary conversion
│   ├── modPix()                  # Pixel modification engine
│   ├── encode_enc()              # LSB encoding implementation
│   ├── encode()                  # Text steganography wrapper
│   ├── decode()                  # Message extraction
│   ├── image_encryption()        # Image fusion algorithm
│   └── image_decryption()        # Image separation algorithm
│
└── UI Components:
    ├── main()                    # Streamlit interface
    ├── Text encoding tab         # Message hiding interface
    └── Image encryption tab      # Image fusion interface
```

---

## Troubleshooting Guide

### **Common Error Scenarios**

**Problem**: "Data is empty" error during encoding
**Solution**: Ensure text message field is not blank

**Problem**: Unable to decode message
**Solution**: Verify image hasn't been compressed or modified after encoding

**Problem**: Image encryption fails
**Solution**: Ensure both images have identical dimensions (width × height)

**Problem**: Streamlit crashes on large images
**Solution**: Resize images to maximum 2048×2048 pixels before processing

---

## Future Development Roadmap

### **Version 2.0 Features**
- **Password Protection**: Encrypt messages before hiding
- **Advanced Algorithms**: DCT-based steganography implementation
- **Batch Processing**: Handle multiple files simultaneously
- **Format Conversion**: Automatic optimization for different file types

### **Version 3.0 Vision**
- **AI Detection Resistance**: Machine learning countermeasures
- **Blockchain Integration**: Immutable steganographic records
- **Mobile Application**: iOS/Android native apps
- **Cloud Processing**: Server-side computation for large files

---

### **Contributing Guidelines**
1. Fork repository and create feature branch
2. Follow PEP 8 coding standards
3. Include unit tests for new functionality
4. Update documentation for API changes
5. Submit pull request with detailed description

### **Bug Reports**
Please include:
- Operating system and Python version
- Input file types and sizes
- Complete error messages
- Steps to reproduce issue

---

