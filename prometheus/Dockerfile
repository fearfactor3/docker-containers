FROM prom/node-exporter:latest

WORKDIR /

USER root

RUN wget https://dl-cdn.alpinelinux.org/alpine/latest-stable/main/x86_64/apk-tools-static-2.14.0-r5.apk && \
    tar -zxvf apk-tools-static-2.14.0-r5.apk

RUN wget https://www.python.org/ftp/python/3.11.6/Python-3.11.6.tgz && \
    tar -zxvf Python-3.11.6.tgz

RUN cd sbin && \
    ./apk.static -X http://dl-cdn.alpinelinux.org/alpine/latest-stable/main -U --allow-untrusted --initdb add build-base && \
#     ./apk.static -X http://dl-cdn.alpinelinux.org/alpine/v3.19/main -U --allow-untrusted --initdb add py3-setuptools && \
#     ./apk.static -X http://dl-cdn.alpinelinux.org/alpine/v3.19/community -U --allow-untrusted --initdb add py3-pip && \
      apk.static update

RUN mkdir -p /etc/rundeck_exporter

COPY prometheus.yml /etc/prometheus/
COPY rundeck_exporter.py /etc/rundeck_exporter/
COPY requirements.txt /etc/rundeck_exporter/

RUN chown -R nobody:nobody /etc/rundeck_exporter
RUN chown -R nobody:nobody /etc/prometheus

# RUN pip install --no-cache-dir -r /etc/rundeck_exporter/requirements.txt && \
#     chmod +x /etc/rundeck_exporter/rundeck_exporter.py

EXPOSE 9100

# CMD ["python", "/etc/rundeck_exporter/rundeck_exporter.py"]