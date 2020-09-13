
FROM python:3.8-buster as builder

WORKDIR /opt/app

COPY requirements.txt /opt/app
RUN pip3 install -r requirements.txt

FROM python:3.8-slim-buster as runner

WORKDIR /app/python/
COPY --from=builder /usr/local/lib/python3.8/site-packages /usr/local/lib/python3.8/site-packages
COPY --from=builder /usr/local/bin/uwsgi /usr/local/bin/uwsgi

RUN apt update \
  && apt install -y libpq5 libxml2 default-mysql-client \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN useradd -r -s /bin/false uwsgiusr
RUN mkdir -p /opt/app/src/logs/app_logs
RUN touch /opt/app/src/logs/server.log
RUN chown -R uwsgiusr /opt/app/src/logs

COPY ./app.py ./constants.py ./uwsgi.ini /app/python/

USER uwsgiusr

CMD ["uwsgi", "--ini", "/app/python/uwsgi.ini"]
