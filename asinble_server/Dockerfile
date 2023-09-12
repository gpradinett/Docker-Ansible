FROM ubuntu

# Install Ansible and required dependencies
RUN apt update -y && apt install software-properties-common -y && \
    add-apt-repository --yes --update ppa:ansible/ansible -y && \
    apt install ansible -y && apt-get install openssh-server -y && \
    apt install systemctl -y && apt install sudo -y

# Define user and password
ARG USER=devops
ARG PASS="devops"
RUN useradd -m -s /bin/bash $USER && echo "$USER:$PASS" | chpasswd

# Grant sudo privileges without a password prompt
RUN echo 'devops ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Start SSH service and enable it on boot
RUN echo 'service ssh start' >> /etc/bash.bashrc && \
    echo 'systemctl enable ssh' >> /etc/bash.bashrc

# Create an Ansible inventory group
RUN echo "[web]" >> /etc/ansible/hosts

# Enable password authentication and disable host key checking
RUN echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config && \
    echo '[defaults]\n remote_tmp = /var/tmp/${USER}/ansible \n host_key_checking = False' >> /etc/ansible/ansible.cfg

# Copy a script to start services
COPY "run.sh" .
RUN ["chmod", "+x", "./run.sh"]

# Set the container command
CMD [ "./run.sh" ,"-D", "FOREGROUND"]
