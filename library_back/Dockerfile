FROM python:latest

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /library_back

COPY requirements.txt /library_back/requirements.txt
RUN pip install -r requirements.txt

COPY . /library_back