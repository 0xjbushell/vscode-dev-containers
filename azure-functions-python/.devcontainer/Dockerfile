FROM mcr.microsoft.com/vscode/devcontainers/python:3.7

# avoids warnings
ENV DEBIAN_FRONTEND=noninteractive

# Install packages
RUN apt-get update \
    && apt-get install -y \
      apt-transport-https \
      ca-certificates \
      curl \
      gnupg-agent \
      software-properties-common

# Configure sources and gpg keys
RUN echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $(lsb_release -cs) main" > /etc/apt/sources.list.d/azure-cli.list \
    && echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-debian-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list \
    && curl -sL https://packages.microsoft.com/keys/microsoft.asc | (OUT=$(apt-key add - 2>&1) || echo $OUT) \
    && curl -fsSL https://download.docker.com/linux/$(lsb_release -is | tr '[:upper:]' '[:lower:]')/gpg | apt-key add - 2>/dev/null \
    && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/$(lsb_release -is | tr '[:upper:]' '[:lower:]') $(lsb_release -cs) stable" \
    && apt-get update

# Install Azure functions, dotnet core, Azure CLI, and Docker CLI
RUN apt-get install -y \
      jq \
      azure-cli \
      dotnet-sdk-3.1 \ 
      azure-functions-core-tools-3 \
      docker-ce-cli \
      pylint \
    && pip install pipenv

RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Prep environment for a good terminal expierence
#
# Create vscode directory
RUN mkdir -p /home/vscode/.local/share \
    && chown -R vscode /home/vscode
#
# Give Azure Functions Core Tools a location to save data
ENV XDG_DATA_HOME=/home/vscode/.local/share
#
# Switch back to dialog
ENV DEBIAN_FRONTEND=dialog