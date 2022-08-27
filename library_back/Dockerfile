FROM postgres

ENV POSTGRES_USER postgres
ENV POSTGRES_PASSWORD password
ENV POSTGRES_DB library_db

COPY init.sql /docker-entrypoint-initdb.d/init.sql
