# 🚀 DeepStream 5.1 on Tesla P4 Setup Guide

This repo contains a **step-by-step installation and troubleshooting guide** for deploying 
NVIDIA DeepStream 5.1 on a Tesla P4 GPU (Ubuntu 18.04).

## 📂 Contents
- [docs/Deepstream5.1onaTeslaP4.pdf](docs/Deepstream5.1onaTeslaP4.pdf) – full detailed guide
- Step-by-step notes for:
  - NVIDIA Drivers (460.x)
  - CUDA 11.1
  - cuDNN 8.1
  - TensorRT 7.2.3
  - DeepStream 5.1
  - GStreamer setup & troubleshooting
  - Kafka integration with librdkafka

## ✅ Why This Repo?
Official docs are scattered across NVIDIA’s site. This is a **single, consolidated guide** based on real-world setup & debugging on Tesla P4.

## 🧑‍💻 Usage
- Read the PDF under `/docs`
- Follow steps in order (drivers → CUDA → cuDNN → TensorRT → DeepStream)
- Use the verification commands to check each installation

## ⚠️ Notes
- Tested on Ubuntu 18.04
- Requires root permissions
- Do not skip verification steps
