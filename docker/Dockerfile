FROM nvidia/cuda:11.8.0-devel-ubuntu20.04

ENV DEBIAN_FRONTEND=noninteractive

ARG ASAP_PATH_ARG
ARG DOCKER_USER_HOME_ARG
ENV ASAP_PATH=${ASAP_PATH_ARG}
ENV DOCKER_USER_HOME=${DOCKER_USER_HOME_ARG}

# Copy cuda11.8 from cuda_stage image
ENV PATH=/usr/local/cuda-11.8/bin:$PATH
ENV TORCH_CUDA_ARCH_LIST="7.0+PTX"
ENV USE_CX11_ABI=0
ENV PRE_CX11_ABI=ON
ENV CUDA_HOME='/usr/local/cuda-11.8/'

# Set up user
WORKDIR ${ASAP_PATH}

# Set default shell to /bin/bash
SHELL ["/bin/bash", "-c"]

# Install system packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    ca-certificates \
    cmake \
    clang \
    curl \
    ffmpeg \
    freeglut3-dev \
    gcc \
    git \
    ibverbs-providers \
    libffi-dev \
    libglfw3 \
    libglfw3-dev \
    libgl1-mesa-glx \
    libgl1-mesa-dev \
    libgles2-mesa-dev \
    libglew-dev \
    libglu1-mesa-dev \
    libibverbs1 \
    libjpeg-dev \
    libosmesa6-dev \
    libpng-dev \
    librdmacm1 \
    libssl-dev \
    libvulkan1 \
    libx11-6 \
    openjdk-8-jdk \
    openssh-client \
    patchelf \
    python3-clang \
    swig \
    unzip \
    vim \
    wget \
    x11-xserver-utils \
    xorg-dev \
    xvfb \
    zip \
    && rm -rf /var/lib/apt/lists/*

# Download the latest installer
ADD https://astral.sh/uv/install.sh /uv-installer.sh

# Run the installer then remove it
RUN sh /uv-installer.sh && rm /uv-installer.sh

# Ensure the installed binary is on the `PATH`
ENV PATH="${DOCKER_USER_HOME}/.cargo/bin/:$PATH"

# Copy the files so that Python packages can be installed using uv
COPY ../pyproject.toml ./
COPY ../uv.lock ./

# Run uv sync
RUN . ${DOCKER_USER_HOME}/.bashrc
