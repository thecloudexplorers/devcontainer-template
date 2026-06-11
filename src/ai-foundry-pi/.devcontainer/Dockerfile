FROM mcr.microsoft.com/powershell:latest

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create non-root user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Install base dependencies + draw.io deps
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    ca-certificates \
    git \
    unzip \
    xvfb \
    libgbm1 \
    libnss3 \
    libxss1 \
    libasound2 \
    libatk-bridge2.0-0 \
    libgtk-3-0 \
    libx11-xcb1 \
    libxcomposite1 \
    libxdamage1 \
    libxrandr2 \
    libxkbcommon0 \
    libxshmfence1 \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js (for Claude Code CLI)
RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    && apt-get install -y nodejs \
    && rm -rf /var/lib/apt/lists/*

# Install Claude Code CLI
RUN npm install -g @anthropic-ai/claude-code

# Install Pi Coding Agent CLI
RUN npm install -g --ignore-scripts @earendil-works/pi-coding-agent

# Install Hermes Agent CLI
RUN curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash

# Install Azure CLI
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash \
    && az extension add --name azure-devops \
    && az extension add --name resource-graph

# Install Bicep
RUN az bicep install

# Install prettier
RUN npm install -g prettier

# Install PowerShell modules
RUN pwsh -NoProfile -Command " \
    Install-Module -Name Terminal-Icons -Force -Scope AllUsers"

USER $USERNAME

RUN npx --yes skills add thecloudexplorers/skills --all -g -y
