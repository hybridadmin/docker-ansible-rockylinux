FROM rockylinux:9
LABEL maintainer="Tinashe Chikomo <tinashe@chikomo.net>"

ENV container docker

ENV pip_packages "ansible yamllint ansible-lint pymysql"

RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

# Install requirements.
#RUN dnf -y install rpm centos-release dnf-plugins-core \
RUN dnf -y update \
# && dnf -y config-manager --set-enabled powertools \
 && dnf --enablerepo=devel -y install \
      epel-release \
      initscripts \
      glibc-langpack-en \
      sudo \
      which \
      hostname \
      libyaml-devel \
      python39 \
      python3-pip \
      python3-pyyaml \
 && dnf clean all

# Install Ansible via Pip.
RUN python3 -m pip install --upgrade pip
RUN python3 -m pip install $pip_packages

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

# Install Ansible inventory file.
RUN mkdir -p /etc/ansible
ADD https://raw.githubusercontent.com/hybridadmin/misc-scripts/master/linux/ansible/hosts /etc/ansible/hosts
ADD https://raw.githubusercontent.com/hybridadmin/misc-scripts/master/linux/ansible/ansible.cfg etc/ansible/ansible.cfg

VOLUME [ "/sys/fs/cgroup" ]

CMD ["/usr/sbin/init"]
