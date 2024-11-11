# Usar la imagen base de Ubuntu
FROM ubuntu:latest

# Establecer el ambiente como no interactivo para evitar problemas al instalar paquetes
ENV DEBIAN_FRONTEND=noninteractive

# Actualizar el sistema y instalar dependencias
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y sudo curl wget gnupg openjdk-11-jdk && \
    apt-get clean

# Instalar Node.js (última versión LTS)
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && \
    apt-get install -y nodejs && \
    node -v && npm -v

# Descargar e instalar Cronicle
RUN curl -s https://raw.githubusercontent.com/jhuckaby/Cronicle/master/bin/install.js | node

# Copiar Pentaho Data Integration a la imagen
COPY Pentaho/data-integration /opt/pentaho/data-integration

# Dar permisos de ejecución a los scripts de Pentaho
RUN chmod +x /opt/pentaho/data-integration/*.sh

# Establecer variables de entorno para Pentaho
ENV PENTAHO_HOME=/opt/pentaho/data-integration
ENV PATH=$PATH:$PENTAHO_HOME

# Exponer el puerto por defecto que usa Cronicle (3012)
EXPOSE 3012

# Comando para iniciar Cronicle y Pentaho
CMD ["bash", "-c", "/opt/cronicle/bin/control.sh setup && sleep 60 && /opt/cronicle/bin/control.sh start && sleep 90 && tail -f /dev/null"]
