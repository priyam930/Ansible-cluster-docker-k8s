FROM alpine:3.15

# Install dependencies
RUN apk add --no-cache \
    python3 \
    openssh \
    sudo

# Create ansible user
RUN adduser -D ansible && \
    echo "ansible:ansible" | chpasswd && \
    mkdir -p /home/ansible/.ssh && \
    chown -R ansible:ansible /home/ansible/.ssh && \
    echo "ansible ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Allow passwordless sudo (already done above)

# Generate default SSH keys (required by sshd)
RUN ssh-keygen -A

# Set correct permissions for sshd
RUN mkdir -p /var/run/sshd

# Switch to ansible user
USER ansible
WORKDIR /home/ansible

# Prepare SSH directory
RUN mkdir -p /home/ansible/.ssh && \
    touch /home/ansible/.ssh/authorized_keys && \
    chmod 700 /home/ansible/.ssh && \
    chmod 600 /home/ansible/.ssh/authorized_keys

# Switch back to root to start sshd
USER root

# Start SSH daemon
CMD ["/usr/sbin/sshd", "-D"]
