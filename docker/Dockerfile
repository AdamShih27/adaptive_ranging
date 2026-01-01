# =============================================================================
# USV Adaptive Ranging System - Dockerfile
# Base: PyTorch 2.7 + CUDA 12.6
# =============================================================================

FROM pytorch/pytorch:2.5.1-cuda12.4-cudnn9-devel

# Prevent interactive prompts during build
ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /app

# -----------------------------------------------------------------------------
# System dependencies
# -----------------------------------------------------------------------------
RUN apt-get update && apt-get install -y \
    git \
    wget \
    curl \
    vim \
    ffmpeg \
    libsm6 \
    libxext6 \
    libgl1-mesa-glx \
    libglib2.0-0 \
    && rm -rf /var/lib/apt/lists/*

# -----------------------------------------------------------------------------
# Upgrade pip and install build tools
# -----------------------------------------------------------------------------
RUN pip install --upgrade pip setuptools wheel

# -----------------------------------------------------------------------------
# Install SAM 3 from local submodule
# -----------------------------------------------------------------------------
COPY external/sam3 /app/external/sam3
RUN cd /app/external/sam3 && pip install -e .

# -----------------------------------------------------------------------------
# Install SAM 3D from local submodule
# -----------------------------------------------------------------------------
COPY external/sam3d /app/external/sam3d
RUN cd /app/external/sam3d && pip install -e .

# -----------------------------------------------------------------------------
# Install project dependencies
# -----------------------------------------------------------------------------
COPY requirements.txt /app/
RUN pip install -r requirements.txt

# -----------------------------------------------------------------------------
# Copy source code
# -----------------------------------------------------------------------------
COPY src/ /app/src/
COPY configs/ /app/configs/
COPY scripts/ /app/scripts/

# -----------------------------------------------------------------------------
# Set Python path
# -----------------------------------------------------------------------------
ENV PYTHONPATH="/app/src:${PYTHONPATH}"

# Default command
CMD ["/bin/bash"]
