FROM rocm/pytorch:rocm6.3.2_ubuntu22.04_py3.10_pytorch_release_2.4.0

ENV DEBIAN_FRONTEND=noninteractive
ENV HSA_OVERRIDE_GFX_VERSION=10.3.0

# Install dependencies
RUN apt update -y && \
    apt install -y \
        zsh \
        git \
        curl \
        wget \
        fonts-powerline && \
    rm -rf /var/lib/apt/lists/*

# Create user 'cu' and set Zsh as the default shell
RUN useradd -m -s /usr/bin/zsh cu && \
    echo "cu ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Basic Provisioning
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt

# Install Oh My Zsh (non-interactive) for 'cu'
RUN sudo -u cu sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended"

# Install Powerlevel10k theme for 'cu'
RUN sudo -u cu git clone --depth=1 https://github.com/romkatv/powerlevel10k.git /home/cu/.oh-my-zsh/custom/themes/powerlevel10k && \
    echo 'ZSH_THEME="powerlevel10k/powerlevel10k"' | sudo -u cu tee -a /home/cu/.zshrc

# Install recommended plugins for 'cu'
RUN sudo -u cu git clone https://github.com/zsh-users/zsh-autosuggestions /home/cu/.oh-my-zsh/custom/plugins/zsh-autosuggestions && \
    sudo -u cu git clone https://github.com/zsh-users/zsh-syntax-highlighting.git /home/cu/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting && \
    echo 'plugins=(git zsh-autosuggestions zsh-syntax-highlighting)' | sudo -u cu tee -a /home/cu/.zshrc

# Set Powerlevel10k config (optional)
RUN echo '[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh' | sudo -u cu tee -a /home/cu/.zshrc

# Ensure correct ownership of home directory
RUN chown -R cu:cu /home/cu

# Default to Zsh
CMD ["zsh"]