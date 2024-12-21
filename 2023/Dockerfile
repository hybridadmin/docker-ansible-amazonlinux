FROM amazonlinux:2023
LABEL maintainer="Tinashe Chikomo <tinashe@chikomo.net>"

ENV container docker

ENV pip_packages "ansible pymysql yamllint ansible-lint"

RUN dnf -y update; dnf clean all

# Install requirements.
RUN dnf makecache \
 && dnf -y install initscripts \
 && dnf -y update \
 && dnf -y install \
      sudo \
      which \
      python3-devel \
      python3-setuptools \
      python3-pip \
 && dnf clean all

# Install Ansible via Pip.
RUN python3 -m pip install $pip_packages

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

# Install Ansible inventory file.
RUN mkdir -p /etc/ansible
ADD https://raw.githubusercontent.com/hybridadmin/misc-scripts/master/linux/ansible/hosts /etc/ansible/hosts
ADD https://raw.githubusercontent.com/hybridadmin/misc-scripts/master/linux/ansible/ansible.cfg etc/ansible/ansible.cfg

RUN cd /lib/systemd/system/sysinit.target.wants/; \
    for i in *; do [ $i = systemd-tmpfiles-setup.service ] || rm -f $i; done

RUN rm -f /lib/systemd/system/multi-user.target.wants/* \
    /etc/systemd/system/*.wants/* \
    /lib/systemd/system/local-fs.target.wants/* \
    /lib/systemd/system/sockets.target.wants/*udev* \
    /lib/systemd/system/sockets.target.wants/*initctl* \
    /lib/systemd/system/basic.target.wants/* \
    /lib/systemd/system/anaconda.target.wants/*


VOLUME [ "/sys/fs/cgroup" ]

CMD ["/usr/sbin/init"]
