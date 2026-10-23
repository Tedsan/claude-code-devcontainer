FROM mcr.microsoft.com/vscode/devcontainers/python:3.11

# Install additional system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js 22 and Claude Code CLI using the nvm bundled in the base image
# (base image: nvm at /usr/local/share/nvm, no Node installed, /usr/local/share/nvm/current/bin on PATH;
#  NVM_SYMLINK_CURRENT=true makes nvm create the "current" symlink)
RUN su vscode -c "umask 0002 && export NVM_DIR=/usr/local/share/nvm NVM_SYMLINK_CURRENT=true && \
    . /usr/local/share/nvm/nvm.sh && \
    nvm install 22 && nvm alias default 22 && \
    npm install -g @anthropic-ai/claude-code"

# Install uv
RUN curl -LsSf https://astral.sh/uv/install.sh | sh

# Create requirements.txt if it doesn't exist
RUN touch /tmp/requirements.txt

# Set working directory
WORKDIR /workspace

# Copy requirements if exists
COPY requirements.txt* ./

# Install Python dependencies
RUN pip install --upgrade pip && \
    pip install \
    jupyter \
    pytest \
    black \
    flake8 \
    mypy \
    isort \
    ruff \
    requests \
    pandas \
    numpy \
    matplotlib \
    seaborn

# Install additional requirements if file exists
RUN if [ -f requirements.txt ]; then pip install -r requirements.txt; fi

# Configure vscode user (may already exist)
RUN if ! id vscode > /dev/null 2>&1; then useradd -m -s /bin/bash vscode; fi && \
    echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Setup uv for vscode user
RUN if [ -d /root/.cargo ]; then cp -r /root/.cargo /home/vscode/.cargo && chown -R vscode:vscode /home/vscode/.cargo; fi && \
    if [ -d /root/.local ]; then cp -r /root/.local /home/vscode/.local && chown -R vscode:vscode /home/vscode/.local; fi && \
    echo 'export PATH="$HOME/.local/bin:$HOME/.cargo/bin:$PATH"' >> /home/vscode/.bashrc

USER vscode

# Update environment variables for vscode user
# (Node/claude come from /usr/local/share/nvm/current/bin, already on PATH in the base image)
ENV PATH="/home/vscode/.local/bin:/home/vscode/.cargo/bin:$PATH"