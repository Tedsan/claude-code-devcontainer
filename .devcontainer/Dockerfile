FROM mcr.microsoft.com/vscode/devcontainers/python:3.11

# Install additional system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js and Claude Code CLI
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash && \
    export NVM_DIR="/usr/local/share/nvm" && \
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && \
    nvm install 22 && \
    npm install -g @anthropic-ai/claude-code

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

# Setup nvm and uv for vscode user
RUN cp -r /usr/local/share/nvm /home/vscode/.nvm && \
    chown -R vscode:vscode /home/vscode/.nvm && \
    if [ -d /root/.cargo ]; then cp -r /root/.cargo /home/vscode/.cargo && chown -R vscode:vscode /home/vscode/.cargo; fi && \
    if [ -d /root/.local ]; then cp -r /root/.local /home/vscode/.local && chown -R vscode:vscode /home/vscode/.local; fi && \
    echo 'export NVM_DIR="/home/vscode/.nvm"' >> /home/vscode/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' >> /home/vscode/.bashrc && \
    echo '[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"' >> /home/vscode/.bashrc && \
    echo 'export PATH="$HOME/.local/bin:$HOME/.cargo/bin:$PATH"' >> /home/vscode/.bashrc

USER vscode

# Update environment variables for vscode user
ENV NVM_DIR="/home/vscode/.nvm"
ENV PATH="$NVM_DIR/versions/node/v22.*/bin:/home/vscode/.local/bin:/home/vscode/.cargo/bin:$PATH"