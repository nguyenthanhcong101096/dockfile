FROM ubuntu:18.04

RUN apt-get update

RUN apt-get install sudo -y \
    && apt-get install git -y \
    && apt-get install nano -y \
    && apt-get install locales -y \
    && apt-get install curl -y \
    && apt-get install openssh-server -y \
    && apt-get install openssh-client -y

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

EXPOSE 22 80 443 8080 3000 50000

USER root

CMD ["/usr/sbin/sshd", "-D"]
