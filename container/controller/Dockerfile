FROM registry.access.redhat.com/ubi8/ubi:8.8-854

# install packages
RUN yum makecache --timer && \
    yum -y install initscripts && \
    yum -y update && \
    yum -y install sudo which hostname python38 openssh-server openssh-clients iputils vim && \
    yum clean all && \
    pip3 install --upgrade --no-cache-dir pip && \
    pip3 install --upgrade --no-cache-dir setuptools && \
    pip3 install --no-cache-dir ansible

# setup ssh server/client
RUN mkdir /var/run/sshd && \
    echo 'root:screencast' | chpasswd && \
    sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd && \
    ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key && \
    ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key && \
    ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key && \
    ssh-keygen -t ed25519 -f /root/.ssh/id_ed25519

# setup ansible
RUN mkdir /etc/ansible

COPY hosts /etc/ansible/

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
