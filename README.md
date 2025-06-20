# Práctica Formativa Obligatoria 3

## Uso e instalación de Kubernetes

Este proyecto contiene una aplicación PHP simple diseñada para ser empaquetada en un contenedor Docker y desplegada en un clúster de Kubernetes.

## Explicación del Dockerfile

El `Dockerfile` contiene las instrucciones para construir la imagen de contenedor de nuestra aplicación.

```dockerfile
# Usa una imagen oficial de PHP con Alpine Linux, que es muy ligera.
FROM php:fpm-alpine

# Crea el directorio estándar para servir archivos web. El flag -p asegura que se creen los directorios padres si no existen.
RUN mkdir -p /var/www/html

# Copia los archivos desde la carpeta local 'src' al directorio '/var/www/html' dentro del contenedor.
# --chown=nobody es una medida de seguridad: cambia el propietario de los archivos a 'nobody', un usuario sin privilegios.
# Esto previene que el proceso del servidor web se ejecute como root.
COPY --chown=nobody src/ /var/www/html/

# Establece el directorio de trabajo por defecto. Los comandos siguientes se ejecutarán desde esta ruta.
WORKDIR /var/www/html

# El comando que se ejecutará cuando el contenedor se inicie.
# Inicia el servidor de desarrollo integrado de PHP.
# -S 0.0.0.0:80 : Escucha en el puerto 80 en todas las interfaces de red del contenedor.
# -t /var/www/html/ : Establece el directorio raíz de los documentos.
CMD ["php", "-S", "0.0.0.0:80", "-t", "/var/www/html/"]
```

## Explicación del deploy-pfo3.yaml

Contiene la definición de un deployment de Kubernetes para desplegar la imagen Docker `pfo3:v1`

```yaml
# Fue creado con el comando:
kubectl create deployment pfo3 --image=pfo3:v1 --dry-run=client -o yaml > deploy-pfo3.yaml

# El YAML es útil para versionar y reutilizar la definición del despliegue.
# Se aplicó con:
kubectl apply -f deploy-pfo3.yaml
```
