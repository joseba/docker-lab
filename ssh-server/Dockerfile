FROM centos:7

ENV USER admin

## install software
RUN yum install -y openssh-server
RUN yum install -y sudo
RUN yum install -y initscripts
RUN yum install -y which

## create admin user
RUN useradd $USER
RUN passwd -f -u $USER
RUN mkdir -p /home/$USER/.ssh; chown $USER /home/$USER/.ssh; chmod 700 /home/$USER/.ssh
ADD ./authorized_keys /home/$USER/.ssh
RUN chown $USER /home/$USER/.ssh/authorized_keys; chmod 600 /home/$USER/.ssh/authorized_keys

## setup sudoers
RUN echo "$USER    ALL=(ALL)       ALL" >> /etc/sudoers.d/$USER

## setup sshd and generate ssh-keys by init script
ADD ./sshd_config /etc/ssh/sshd_config
RUN /usr/sbin/sshd-keygen

EXPOSE 22

CMD    ["/usr/sbin/sshd", "-D"]
