FROM tamphubkdn/ubuntu1604-rvm-mysql
MAINTAINER phunt <phunt@runsystem.net>

# Env vars define
ENV ROOT_PASSWD="phunt@123"
ENV RUBY_VERSION=2.5.0
ENV RAILS_VERSION=5.2.0
ENV PATH /usr/local/rvm/bin:$PATH

# To run dpkg (behind other tools like Apt) without interactive dialogue, you can set one environment variable as
ENV DEBIAN_FRONTEND noninteractive
# apt-utils seems missing and warnings are shown, so we install it.
RUN apt-get update -q -q && \
    apt-get install --yes --force-yes apt-utils tzdata locales && \
    echo 'UTC' > /etc/timezone && \
    rm /etc/localtime && \
    dpkg-reconfigure tzdata && \
    apt-get upgrade --yes --force-yes

# Install Nokogiri on a brand new Ubuntu system
RUN apt-get -y install \
    build-essential \
    patch \
    ruby-dev \
    zlib1g-dev \
    liblzma-dev \
    openssh-server \
    supervisor

# Install ruby
RUN /bin/bash -l -c 'rvm requirements'
RUN /bin/bash -l -c 'rvm install $RUBY_VERSION'
RUN /bin/bash -l -c 'rvm use $RUBY_VERSION --default'
# RUN /bin/bash -l -c 'rvm rubygems current'
# Install bundler
RUN /bin/bash -l -c 'gem install bundler --no-doc --no-ri'

# Install rails
RUN gem install rails -v $RAILS_VERSION

# Config ssh
RUN mkdir /var/run/sshd && \
    echo "root:${ROOT_PASSWD}" | chpasswd && \
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

# Config supervisord
COPY /supervisord/supervisord.conf /etc/supervisor/supervisord.conf
COPY /supervisord/conf.d /etc/supervisor/conf.d

# Clean
RUN rm -rf /var/lib/apt/lists/*

EXPOSE 22

CMD ["/usr/bin/supervisord", "--configuration=/etc/supervisor/supervisord.conf"]
