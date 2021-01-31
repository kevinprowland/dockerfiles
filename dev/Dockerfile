FROM ubuntu:18.04
# Install prerequisites
RUN apt-get update && apt-get install -y \
build-essential \
curl \
cpanminus \
vim \
git \
openssh-server

RUN cpanm Pod::Usage

RUN printf \
" \
# texlive.profile written on Sat Jan 30 22:54:31 2021 UTC\n \
# It will NOT be updated and reflects only the\n \
# installation profile at installation time.\n \
selected_scheme scheme-basic\n \
TEXDIR /usr/local/texlive/2020\n \
TEXMFCONFIG ~/.texlive2020/texmf-config\n \
TEXMFHOME ~/texmf\n \
TEXMFLOCAL /usr/local/texlive/texmf-local\n \
TEXMFSYSCONFIG /usr/local/texlive/2020/texmf-config\n \
TEXMFSYSVAR /usr/local/texlive/2020/texmf-var\n \
TEXMFVAR ~/.texlive2020/texmf-var\n \
binary_x86_64-linux 1\n \
instopt_adjustpath 0\n \
instopt_adjustrepo 1\n \
instopt_letter 0\n \
instopt_portable 0\n \
instopt_write18_restricted 1\n \
tlpdbopt_autobackup 1\n \
tlpdbopt_backupdir tlpkg/backups\n \
tlpdbopt_create_formats 1\n \
tlpdbopt_desktop_integration 1\n \
tlpdbopt_file_assocs 1\n \
tlpdbopt_generate_updmap 0\n \
tlpdbopt_install_docfiles 1\n \
tlpdbopt_install_srcfiles 1\n \
tlpdbopt_post_code 1\n \
tlpdbopt_sys_bin /usr/local/bin\n \
tlpdbopt_sys_info /usr/local/share/info\n \
tlpdbopt_sys_man /usr/local/share/man\n \
tlpdbopt_w32_multi_user 1"\n \
> texlive.profile

RUN curl -L http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz | tar xzf -
RUN ./install-tl-20210130/install-tl \
-profile texlive.profile \
-repository http://ctan.mirrors.hoobly.com/systems/texlive/tlnet/

RUN echo "PATH=/usr/local/texlive/2020/bin/x86_64-linux:${PATH}" >> ${HOME}/.bashrc

ARG ssh_private_key

RUN mkdir -p ${HOME}/.ssh && \
    chmod 0700 ${HOME}/.ssh && \
    ssh-keyscan github.com > ${HOME}/.ssh/known_hosts

RUN echo "$ssh_private_key" > ${HOME}/.ssh/id_rsa && \
    echo "$ssh_public_key" > ${HOME}/.ssh/id_rsa.pub && \
    chmod 600 ${HOME}/.ssh/id_rsa && \
    chmod 600 ${HOME}/.ssh/id_rsa.pub

RUN cd ${HOME} 

RUN git clone git@github.com:kevinprowland/resumes.git

RUN /usr/local/texlive/2020/bin/x86_64-linux/tlmgr --verify-repo=none install mdwtools relsize xcolor tex-gyre microtype units enumitem

RUN cd /usr/local/texlive/2020/bin/x86_64-linux; ln -s dvipdfm dvipdf

CMD /bin/bash
