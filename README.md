# Hecho por: Hugo Mata
# Proyecto de Proxy Inverso y Balanceo de Carga

Este proyecto implementa una infraestructura de red robusta utilizando Docker y Nginx. Consiste en un proxy inverso que actúa como balanceador de carga para dos servidores web que sirven el mismo contenido de forma sincronizada.

## Requisitos Previos

Antes de comenzar, asegúrate de tener instalado en tu sistema:
*   [Docker](https://docs.docker.com/get-docker/)
*   [Docker Compose](https://docs.docker.com/compose/install/)

## Estructura del Proyecto

*   `proxy-exam/`
    *   `docker-compose.yml`: Configuración de la orquestación de los contenedores.
    *   `nginx.conf`: Configuración del proxy inverso y reglas de balanceo.
    *   `html/`: Directorio que contiene el sitio web compartido (HTML, imágenes, vídeos).
    *   `README.md`: Este archivo.

## Instalación y Despliegue

Sigue estos pasos para poner en marcha el entorno:

1.  **Navega al directorio del proyecto:**
    ```bash
    cd proxy-exam
    ```

2.  **Levanta los contenedores:**
    ```bash
    docker compose up -d
    ```

Este comando descargará las imágenes oficiales de Nginx, creará una red interna aislada y arrancará los tres servicios (el proxy y los dos servidores web).

## Verificación

Una vez desplegado, puedes acceder al sitio web abriendo tu navegador en:
`http://localhost`

### Cómo comprobar el balanceo de carga
El proxy añade una cabecera personalizada llamada `X-Backend-Server` que te permite saber qué servidor interno está respondiendo. Puedes verificarlo con el siguiente comando en la terminal:

```bash
curl -I http://localhost
```

Busca la línea que empieza por `X-Backend-Server`. Si refrescas varias veces, verás cómo alterna entre las IPs internas de los dos servidores web.

## Detalles Técnicos

*   **Puerto 80**: El proxy es el único servicio accesible desde el host a través del puerto 80.
*   **Aislamiento**: Los servidores web (`web1` y `web2`) están en una red interna privada y no pueden ser accedidos directamente desde el host, solo a través del proxy.
*   **Volumen Compartido**: Ambos servidores web montan la carpeta `./html` en modo lectura, lo que garantiza que el contenido sea idéntico en ambos nodos.

## Personalización

Para cambiar el contenido del sitio web, simplemente modifica los archivos dentro de la carpeta `html/`. No es necesario reiniciar los contenedores, ya que los cambios se reflejan en tiempo real gracias al volumen montado.
