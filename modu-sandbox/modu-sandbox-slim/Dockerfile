FROM python:3.14.2-slim

# Update package list and install required dependencies
RUN apt-get update && apt-get install -y \
    curl \
    sudo \
    bash \
    python3-pip \
    python3-venv \
    ripgrep \
    && rm -rf /var/lib/apt/lists/*

# Change default shell to bash
RUN chsh -s /bin/bash

SHELL ["/bin/bash", "-c"]

# Install pipx and uv
RUN python3 -m pip install pipx && pipx ensurepath && pipx install uv

# Install Python essential pip packages
RUN python3 -m pip install matplotlib pandas numpy

# Create the modu user and configure sudo access
RUN useradd -m modu && echo "modu ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/91-modu

ENV NVM_DIR=/usr/local/nvm
ENV NODE_VERSION=24.12.0
RUN mkdir -p $NVM_DIR

# Install nvm with node and npm
RUN curl https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# install node and npm
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

# add node and npm to path so the commands are available
ENV NODE_PATH=$NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH=$NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN npm install -g ts-node typescript typescript-language-server

# Create directory for computer use plugin and set ownership
RUN mkdir -p /usr/local/lib && chown modu:modu /usr/local/lib

# Switch to modu user
USER modu

# Keep the container running indefinitely
ENTRYPOINT [ "sleep", "infinity" ]
