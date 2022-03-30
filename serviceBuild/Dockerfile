FROM ubuntu:latest as builder

ARG DEBIAN_FRONTEND=noninteractive

WORKDIR /server

RUN apt-get -y update && \
apt-get -y install git build-essential ffmpeg libpcre3 libpcre3-dev libssl-dev zlib1g-dev wget \
&& rm -rf /var/lib/apt/lists/*


RUN git clone https://github.com/sergey-dryabzhinsky/nginx-rtmp-module.git

RUN wget http://nginx.org/download/nginx-1.21.6.tar.gz \
&& tar -xf nginx-1.21.6.tar.gz

WORKDIR /server/nginx-1.21.6

RUN ./configure --prefix=/server --with-http_ssl_module --add-module=../nginx-rtmp-module \
&& make -j 1 \
&& make install

FROM ubuntu:latest as runner

ARG DEBIAN_FRONTEND=noninteractive

WORKDIR /server

RUN apt-get -y update && \
apt-get -y install ffmpeg libpcre3 libpcre3-dev libssl-dev zlib1g-dev

COPY --from=builder /server ./

EXPOSE 8080
EXPOSE 1935

CMD [ "./sbin/nginx", "-g daemon off;" ]
