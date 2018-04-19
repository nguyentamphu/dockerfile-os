FROM tamphubkdn/ubuntu1604-rbenv-mysql:latest
MAINTAINER phunt <phunt@runsystem.net>

# Env vars define
ENV ROOT_PASSWD="phunt@123"
ENV RUBY_VERSION=2.5.0
ENV PATH /root/.rbenv/bin:$PATH

# Install ruby
RUN rbenv install $RUBY_VERSION
RUN rbenv global $RUBY_VERSION

RUN apt-get -y install rubygems
RUN gem install bundler

# Install common packages
RUN apt-get -y install \
    openssh-server \
    supervisor

# Config ssh
RUN mkdir /var/run/sshd && \
    echo "root:${ROOT_PASSWD}" | chpasswd && \
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

# Config supervisord
ADD /supervisord/supervisord.conf /etc/supervisor/supervisord.conf
ADD /supervisord/conf.d /etc/supervisor/conf.d

# Clean
RUN rm -rf /var/lib/apt/lists/*

EXPOSE 22

CMD ["/usr/bin/supervisord", "--configuration=/etc/supervisor/supervisord.conf"]
