FROM debian:8.2
COPY requirements.* /home/debian/app/
RUN cd /home/debian/app && \
  apt-get update && apt-get install -y sudo python3 wget && \
  wget https://raw.github.com/pypa/pip/master/contrib/get-pip.py && \
  python3 get-pip.py && \
  rm get-pip.py && \
  useradd -m debian && \
  echo debian:password | chpasswd && \
  gpasswd -a debian sudo && \
  pip3 install -r requirements.txt && \
  wget http://nginx.org/keys/nginx_signing.key && \
  apt-key add nginx_signing.key && \
  echo "deb http://nginx.org/packages/debian/ jessie nginx" >> /etc/apt/sources.list && \
  echo "deb-src http://nginx.org/packages/debian/ jessie nginx" >> /etc/apt/sources.list && \
  apt-get update && \
  apt-get install nginx && \
  rm nginx_signing.key && \
  rm -rf /var/lib/apt/lists/* && \
  chown -R debian:debian /home/debian
USER debian
ONBUILD COPY . /home/debian/app/src/
ONBUILD WORKDIR /home/debian/app/src
ONBUILD EXPOSE 8888
ONBUILD RUN nginx -c nginx.conf
ONBUILD ENTRYPOINT python3 main.py
