FROM python:2-alpine3.6
LABEL maintainer "Andre Peters <andre.peters@servercow.de>"

RUN apk add -U --no-cache iptables ip6tables
RUN pip install docker flask flask-restful

COPY server.py /
CMD ["python2", "-u", "/server.py"]
