FROM ubuntu

# Install necessary packages
RUN apt-get update -y && \
    apt-get install -y net-tools openssh-server sudo && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set user and password
ARG USER=devops
ARG PASS=devops
RUN useradd -m -s /bin/bash $USER && \
    echo "$USER:$PASS" | chpasswd

# Add user to sudoers
RUN echo "$USER ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Configure SSH
RUN echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config

CMD service ssh start && /bin/bash
