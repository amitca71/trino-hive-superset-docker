FROM apache/superset:latest
# Switching to root to install the required packages
USER root
# Example: installing the MySQL driver to connect to the metadata database
# if you prefer Postgres, you may want to use `psycopg2-binary` instead
RUN pip install --upgrade pip
RUN pip install mysqlclient
RUN pip install sqlalchemy-trino
RUN apt update -y
RUN apt install alien -y
RUN apt install -y unixodbc unixodbc-dev
RUN pip install sqlalchemy_dremio
RUN wget https://download.dremio.com/odbc-driver/dremio-odbc-LATEST.x86_64.rpm && alien -i --scripts dremio-odbc-LATEST.x86_64.rpm
# Switching back to using the `superset` user
USER superset
