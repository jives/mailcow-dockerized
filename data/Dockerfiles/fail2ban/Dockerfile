FROM python:2-alpine3.6
LABEL maintainer "Andre Peters <andre.peters@servercow.de>"

RUN apk add -U --no-cache iptables ip6tables
RUN pip install redis ipaddress

COPY logwatch.py /
CMD ["python2", "-u", "/logwatch.py"]
