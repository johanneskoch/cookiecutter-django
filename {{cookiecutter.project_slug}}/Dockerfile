FROM python:3.8-slim-buster

ENV PYTHONUNBUFFERED 1

RUN apt-get clean && apt-get update \
  # build-essential metapackage install: make, gcc, g++,
  && apt-get install -y build-essential \
  # required to translate
  # && apt-get install -y gettext python3-dev \
  # shared dependencies of Pillow, pylibmc
  && apt-get install -y zlib1g-dev \
  # Postgresql and psycopg2 dependencies
  && apt-get install -y libpq-dev \
  # Pillow dependencies
  # && apt-get install -y libtiff5-dev libjpeg62-turbo-dev libfreetype6-dev liblcms2-dev libwebp-dev \
  # django-extensions
  && apt-get install -y graphviz-dev

# Requirements are installed here to ensure they will be cached.
COPY ./requirements /requirements
RUN pip install -r /requirements/local.txt

COPY ./compose/local/django/entrypoint /entrypoint
RUN sed -i 's/\r$//g' /entrypoint
RUN chmod +x /entrypoint

COPY ./compose/local/django/start /start
RUN sed -i 's/\r$//g' /start
RUN chmod +x /start
{% if cookiecutter.use_celery == "y" %}
COPY ./compose/local/django/celery/worker/start /start-celeryworker
RUN sed -i 's/\r$//g' /start-celeryworker
RUN chmod +x /start-celeryworker

COPY ./compose/local/django/celery/beat/start /start-celerybeat
RUN sed -i 's/\r$//g' /start-celerybeat
RUN chmod +x /start-celerybeat

COPY ./compose/local/django/celery/flower/start /start-flower
RUN sed -i 's/\r$//g' /start-flower
RUN chmod +x /start-flower
{% endif %}
WORKDIR /app

ENTRYPOINT ["/entrypoint"]
