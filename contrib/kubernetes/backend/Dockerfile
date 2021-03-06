FROM python:3.6
ENV PYTHONUNBUFFERED 1

ARG VERSION

USER root

RUN yum install -y \
    mongodb-org-tools \ 
    postgresql-client \
    gettext \
    && yum clean all -y
   

RUN pip install --no-cache-dir virtualenv && \
    virtualenv --python=python3 /venv && \
    /venv/bin/pip install uwsgi alerta alerta-server==$VERSION

ENV PATH $PATH:/venv/bin

WORKDIR /app

RUN chgrp -R 0 /app /venv && \
    chmod -R g=u /app /venv && \
    useradd -u 2001 -g 0 alerta

USER 2001

COPY wsgi.py /app/wsgi.py
COPY uwsgi.ini /app/uwsgi.ini

ENV ALERTA_SVR_CONF_FILE /app/alertad.conf
ENV ALERTA_CONF_FILE /app/alerta.conf
ENV BASE_URL /
ENV INSTALL_PLUGINS ""

EXPOSE 8080

COPY docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["uwsgi", "--ini", "/app/uwsgi.ini"]
