FROM amazonlinux:2
LABEL maintainer="Tinashe Chikomo <tinashe@chikomo.net>"

ENV container docker

ENV pip_packages "ansible pymysql yamllint ansible-lint"

RUN yum -y update; yum clean all

# Install requirements.
RUN yum makecache fast \
 && yum -y install deltarpm epel-release initscripts \
 && yum -y update \
 && yum -y install \
      sudo \
      which \
      python3-dev \
      python3-setuptools \
      python3-pip \
 && yum clean all

# Install Ansible via Pip.
RUN python3 -m pip install --upgrade pip
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
