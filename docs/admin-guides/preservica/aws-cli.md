# AWS CLI

## Overview

> **Purpose:** The AWS Command Line Interface (CLI) is a unified tool to manage AWS services. In the context of digital archiving, it's primarily used for uploading large files to Preservica's PUT area. The CLI provides a more reliable and efficient way to handle large file transfers compared to web interfaces.

## Installation
1. Download the AWS CLI from the [official website](https://docs.aws.amazon.com/cli/index.html)
2. Follow the installation instructions for your operating system:

      - Linux: Use package manager or install from source

## Configuration

After installation, configure the AWS CLI with your Preservica credentials:

**Command:**
```bash
aws configure
```

**You'll need to enter:**
- AWS Access Key ID (from Preservica Logins page)
- AWS Secret Access Key (from Preservica Logins page)
- Default region name (use the one specified in Preservica)
- Default output format (json or text)

## Usage Examples

### List Directory Contents
```bash
aws s3 ls s3://com.preservica.icaew.put.holding/    
```

### Upload a Single File
```bash
aws s3 cp '/home/digital-archivist/Downloads/20220719-ICAEW-com-media-library.zip' s3://com.preservica.icaew.put.holding/20220719-ICAEW-com-media-library.zip
```

### Upload a Directory
```bash
aws s3 cp --recursive local_directory s3://com.preservica.icaew.put.holding/NewFolder/
```

### List Objects with Metadata
```bash
aws s3api list-objects --bucket com.preservica.icaew.export
```