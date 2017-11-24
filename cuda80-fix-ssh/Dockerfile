# cuda80-fix-ssh
#
# VERSION               0.0.1

FROM      allenyllee/cuda80-fix
LABEL     maintainer="allen7575@gmail.com"

############
# update package list
############
RUN apt update


### install vim
RUN apt install -y vim

### install & start sshd server
### for ssh login & X11 forward use
RUN apt install -y ssh

### enable ssh root login
RUN sed -i "s/PermitRootLogin/# PermitRootLogin/g" /etc/ssh/sshd_config
RUN echo "PermitRootLogin yes" >> /etc/ssh/sshd_config

### change password with username:password
RUN echo root:root | chpasswd

### first start to preserve all SSH host keys
RUN service ssh start

### install sshfs
RUN apt install -y sshfs

###
### add nvcc PATH for ssh interactive
### unix - How do I set $PATH such that `ssh user@host command` works? - Stack Overflow 
### https://stackoverflow.com/questions/940533/how-do-i-set-path-such-that-ssh-userhost-command-works
###
#RUN sed -i '/interactively/ i export PATH=$PATH:/usr/local/cuda-8.0/bin\n' /root/.bashrc

### Newby question to CUDA container and ssh · Issue #36 · NVIDIA/nvidia-docker 
### https://github.com/NVIDIA/nvidia-docker/issues/36
RUN echo "export PATH=\$PATH" >> /etc/profile && \
    echo "ldconfig" >> /etc/profile

##############
# upgrade
##############
RUN apt upgrade -y

##############
# cleanup
##############
# debian - clear apt-get list - Unix & Linux Stack Exchange
# https://unix.stackexchange.com/questions/217369/clear-apt-get-list
#
# bash - autoremove option doesn't work with apt alias - Ask Ubuntu
# https://askubuntu.com/questions/573624/autoremove-option-doesnt-work-with-apt-alias
#
RUN apt-get autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


### make dir for mount point use
RUN mkdir ~/client-sshfs-project


CMD    ["bash"]


