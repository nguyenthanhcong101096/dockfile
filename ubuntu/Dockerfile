FROM ubuntu:18.04

RUN apt-get update

RUN apt-get install sudo -y
RUN apt-get install git -y
RUN apt-get install nano -y
RUN apt-get install locales -y
RUN apt-get install curl -y
RUN apt-get install openssh-server -y
RUN apt-get install openssh-client -y

RUN mkdir /var/run/sshd \
    systemctl enable ssh

RUN ssh-keygen -A \
    && adduser app \
    && echo "app:password" | chpasswd

RUN echo "PasswordAuthentication no" >> /etc/ssh/sshd_config \
    && echo "PermitRootLogin no" >> /etc/ssh/sshd_config \
    && echo "app ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

USER app

WORKDIR /home/app

RUN mkdir -p .ssh \
    && sudo -S -k locale-gen en_US en_US.UTF-8 \
    && echo 'export LC_ALL="en_US.UTF-8"' >> ~/.bashrc

ADD authorized_keys /home/app/.ssh/

RUN sudo apt install -y apt-transport-https ca-certificates curl software-properties-common \
    && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - \
    && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" \
    && sudo apt -y update \
    && sudo apt install -y docker-ce \
    && sudo usermod -aG docker app

# RUN curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add \
#     && sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main" \
#     && sudo apt-get install -y kubeadm kubelet kubectl \
#     && sudo sed -i '/swap/d' /etc/fstab \
#     && sudo mkdir .kube

# COPY config /home/app/.kube/

EXPOSE 22 80 443 8080 3000 50000

USER root

CMD ["/usr/sbin/sshd", "-D"]