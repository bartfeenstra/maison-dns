FROM nginx:1.14-alpine

RUN apk add --no-cache --virtual=.build-dependencies gcc libffi-dev python-dev musl-dev openssl-dev && \
    apk add --no-cache bash esh python2 py2-pip && \
    pip2 install certbot certbot-nginx && \
    apk del .build-dependencies && rm -rf /var/cache/apk/* && \
    wget -O /tmp/consul-template.tar.gz https://releases.hashicorp.com/consul-template/0.19.5/consul-template_0.19.5_linux_amd64.tgz && \
    tar -xzf /tmp/consul-template.tar.gz -C /usr/local/bin && \
    rm -r /tmp/* && \
    rm /etc/nginx/conf.d/default.conf

COPY ./conf/nginx.conf /etc/nginx/
COPY ./conf/conf.d/* /etc/nginx/conf.d/
COPY ./run /maison/run
COPY ./conf/web-services.ctmpl /maison/web-services.ctmpl
COPY ./reload-certbot /maison/reload-certbot
COPY ./reload-nginx /maison/reload-nginx
RUN chmod ug+rx /maison/run /maison/reload-certbot /maison/reload-nginx

CMD ["/maison/run"]

EXPOSE 80 443
