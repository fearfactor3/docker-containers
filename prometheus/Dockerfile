FROM prom/prometheus

COPY prometheus.yml /etc/prometheus/

WORKDIR /

USER root

RUN wget https://dl-cdn.alpinelinux.org/alpine/latest-stable/main/x86_64/apk-tools-static-2.14.0-r5.apk && \
    tar -zxvf apk-tools-static-2.14.0-r5.apk

RUN cd sbin && \
    ./apk.static -X http://dl-cdn.alpinelinux.org/alpine/latest-stable/main -U --allow-untrusted --initdb add python3 && \
    ./apk.static -X http://dl-cdn.alpinelinux.org/alpine/v3.19/community -U --allow-untrusted --initdb add py3-setuptools && \
    ./apk.static -X http://dl-cdn.alpinelinux.org/alpine/v3.19/main -U --allow-untrusted --initdb add py3-pip && \
    apk.static update

RUN mkdir -p /etc/rundeck_exporter/ && \
    chown -R nobody:group /etc/rundeck_exporter/

COPY prometheus/rundeck_exporter.py /etc/rundeck_exporter/ && \
     prometheus/requirements.txt /etc/rundeck_exporter/

RUN pip install --no-cache-dir -r /etc/rundeck_exporter/requirements.txt && \
    chmod +x /etc/rundeck_exporter/rundeck_exporter.py

USER nobody

CMD ["python", "/etc/rundeck_exporter/rundeck_exporter.py"]