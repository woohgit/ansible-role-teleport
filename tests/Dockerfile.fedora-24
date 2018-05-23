FROM fedora:24
RUN dnf -y install ansible

# Install python-netaddr
RUN dnf -y install python-netaddr

# Install Ansible inventory file
RUN echo "[local]\nlocalhost ansible_connection=local" > /etc/ansible/hosts
