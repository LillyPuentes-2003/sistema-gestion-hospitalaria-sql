FROM liquibase/liquibase:4.25

# Instalar driver de PostgreSQL
USER root
RUN lpm add postgresql --global
USER liquibase

# Establecer directorio de trabajo
WORKDIR /liquibase/changelog

CMD ["--help"]