FROM registry.fedoraproject.org/fedora:36

LABEL maintainer="Ilia Baktherev"

LABEL description="A docker image for running Ansible roles testing"

# Create `ansible` user with sudo permissions and membership in `DEPLOY_GROUP`
ENV ANSIBLE_USER=ansible SUDO_GROUP=wheel DEPLOY_GROUP=deployer

RUN dnf update -y && dnf install -y systemd openssh-server \
  && dnf clean all

RUN set -xe \&& groupadd -r ${ANSIBLE_USER} \
  && groupadd -r ${DEPLOY_GROUP} \
  && useradd -m -g ${ANSIBLE_USER} ${ANSIBLE_USER} \
  && usermod -aG ${SUDO_GROUP} ${ANSIBLE_USER} \
  && usermod -aG ${DEPLOY_GROUP} ${ANSIBLE_USER} \
  && sed -i "/^%${SUDO_GROUP}/s/ALL\$/NOPASSWD:ALL/g" /etc/sudoers

RUN echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config \
  && echo "PermitRootLogin yes" >> /etc/ssh/sshd_config

RUN ssh-keygen -A

RUN systemctl enable sshd.service

CMD [ "/usr/sbin/init" ]