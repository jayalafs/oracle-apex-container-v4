# Utiliza la imagen oficial de Oracle Database Free
FROM container-registry.oracle.com/database/free:latest

# Cambiar al usuario root para instalar dependencias
USER root

# Corregir el repositorio YUM de Oracle Linux 8
RUN printf "[ol8_developer]\n\
name=Oracle Linux 8 Development Packages (\$basearch)\n\
baseurl=https://yum.oracle.com/repo/OracleLinux/OL8/developer/\$basearch/\n\
gpgcheck=1\n\
enabled=0\n\
gpgkey=https://yum.oracle.com/RPM-GPG-KEY-oracle-ol8\n\
\n\
[ol8_baseos_latest]\n\
name=Oracle Linux 8 BaseOS Latest (\$basearch)\n\
baseurl=https://yum.oracle.com/repo/OracleLinux/OL8/baseos/latest/\$basearch/\n\
gpgcheck=1\n\
enabled=1\n\
gpgkey=https://yum.oracle.com/RPM-GPG-KEY-oracle-ol8\n\
\n\
[ol8_appstream]\n\
name=Oracle Linux 8 Application Stream (\$basearch)\n\
baseurl=https://yum.oracle.com/repo/OracleLinux/OL8/appstream/\$basearch/\n\
gpgcheck=1\n\
enabled=1\n\
gpgkey=https://yum.oracle.com/RPM-GPG-KEY-oracle-ol8\n" \
> /etc/yum.repos.d/oracle-linux-ol8.repo

# Instalacion de paquetes
RUN dnf update -y && \
    dnf install -y sudo nano java-17-openjdk zip unzip curl && \
    dnf clean all
    
# Crear los directorios necesarios
RUN mkdir -p /home/oracle/software/apex \
    /home/oracle/software/ords \
    /home/oracle/scripts/startup \
    /etc/ords/config \
    /home/oracle/logs

# Copiar los scripts al contenedor
COPY ./src/00_start_apex_ords_installer.sh /home/oracle/scripts/startup/00_start_apex_ords_installer.sh
COPY ./src/unattended_apex_install_23c.sh /home/oracle/unattended_apex_install_23c.sh

# Dar permisos de ejecución a los scripts
RUN chmod +x /home/oracle/scripts/startup/00_start_apex_ords_installer.sh
RUN chmod +x /home/oracle/unattended_apex_install_23c.sh

# Ejecutar el script de instalación de APEX durante la construcción
RUN /home/oracle/scripts/startup/00_start_apex_ords_installer.sh

# Exponer los puertos necesarios
EXPOSE 1521 5500 8080 8443 22

# El CMD original de la imagen oficial ya levanta la base de datos