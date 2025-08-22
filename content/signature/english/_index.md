---
title: "GroupDocs.Signature Cloud API Documentation"
id: "main-documentation"
url: /signature/
productName: "GroupDocs.Signature Cloud"
weight: 1
description: "Complete developer guide for GroupDocs.Signature Cloud API covering all data structures, API methods, and workflows for document signature operations including signing, verifying, searching, updating, and deleting signatures."
keywords: "signature api, document signing, digital signatures, barcode signatures, qr code signatures, text signatures, image signatures, stamp signatures, verify signatures, search signatures, update signatures, delete signatures, cloud api"
toc: True
---

# GroupDocs.Signature Cloud API Documentation

## Overview

Welcome to the GroupDocs.Signature Cloud API documentation. This comprehensive guide covers all data structures and API methods for document signature operations including signing, verifying, searching, updating, and deleting signatures in various document formats.

## GroupDocs.Signature Tutorials

- [Advanced Features](/advanced-features/) - Learn to implement advanced document signing features with our step-by-step tutorials for GroupDocs.Signature Cloud API.

- [Document Beginner Guide Tutorials](./beginner-guide/) - Learn step-by-step how to use GroupDocs.Signature Cloud API with these beginner tutorials for developers.


## API Workflow

### Basic Operations

1. **Document Information**
   ```
   InfoSettings → Info API → InfoResult
   ```

2. **Document Signing**
   ```
   SignSettings → Sign API → SignResult
   ```

3. **Signature Verification**
   ```
   VerifySettings → Verify API → VerifyResult
   ```

4. **Signature Search**
   ```
   SearchSettings → Search API → SearchResult
   ```

5. **Signature Update**
   ```
   UpdateSettings → Update API → UpdateResult
   ```

6. **Signature Deletion**
   ```
   DeleteSettings → Delete API → DeleteResult
   ```

### Supported Signature Types

- **Text Signatures**: Custom text with formatting options
- **Image Signatures**: Upload and position image files
- **Digital Signatures**: Cryptographic document signing
- **Barcode Signatures**: Various barcode formats (Code128, etc.)
- **QR Code Signatures**: QR codes with custom data
- **Stamp Signatures**: Multi-line stamp designs

### Document Format Support

The API supports various document formats including:
- Microsoft Word (DOC, DOCX)
- PDF documents
- Excel spreadsheets
- PowerPoint presentations
- Image formats

## Getting Started

1. Choose the appropriate Settings structure for your operation
2. Configure required parameters (FilePath is mandatory)
3. Set optional parameters as needed (storage, password, etc.)
4. Make API call with the settings object
5. Process the corresponding Result structure response

## Key Features

- **Multi-format Support**: Work with various document types
- **Flexible Positioning**: Precise signature placement
- **Rich Styling Options**: Fonts, colors, borders, backgrounds
- **Batch Operations**: Multiple signatures in single request
- **Secure Operations**: Password-protected document support
- **Cloud Storage**: Integration with cloud storage services
