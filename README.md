# Prueba de DevOps: Provisionar un entorno con Terraform
Para esta prueba, deberás preparar un pipeline de **Terraform** que provisionará la infraestructura de un entorno de, al menos, 3 máquinas.

Dos de las máquinas alojarán sendos servidores web **nginx** preparados para recibir peticiones HTTP con balanceo de carga (**upstreams**).

Además de nginx, cada una de ellas llevará instalado **PHP-FPM** para poder servir páginas dinámicas.  
La otra máquina será el cliente, cuya única función será la de probar que existe comunicación con el servidor.

## Características de las máquinas
El proveedor lo dejamos a tu elección.  
Puedes usar una cuenta gratuita en el que más te convenga: AWS, Google Cloud... tienes una lista interminable para Terraform :)  
Todas las instancias serán idénticas, con las siguientes características:
- Tendrán acceso SSH mediante par de claves.
- El SO será Debian 10.
- Estarán localizadas en la misma región del proveedor que elijas.
- Tendrán habilitada la red privada para poder comunicarse entre ellas.

## Características de nginx
El servidor web de **nginx** tiene ciertas peculiaridades.  
No lo instalaremos desde el paquete de la distro, sino que lo compilaremos usando dos módulos (plugins) no suministrados por la distribución nginx oficial:
- [Nginx Push Stream](https://github.com/wandenberg/nginx-push-stream-module)
- [Headers More](https://github.com/openresty/headers-more-nginx-module)

Además de estos dos módulos no oficiales, compilamos el nginx solo con los siguientes módulos activados:
- ipv6 (`--with-ipv6`)
- stream (`--with-stream`)
- http\_stub\_status\_module (`--with-http_stub_status_module`)
- http\_realip\_module (`--with-http_realip_module`)
- http\_ssl\_module (`--with-http_ssl_module`)
- http\_v2\_module (`--with-http_v2_module`)
- http\_gzip\_static\_module (`--with-http_gzip_static_module`)
  
Además de eliminar de la compilación los siguientes módulos:
- mail\_pop3\_module (`--without-mail_pop3_module`)
- mail\_imap\_module (`--without-mail_imap_module`)
- mail\_smtp\_module (`--without-mail_smtp_module`)

## Requisitos principales:
- Construir el pipeline de Terraform para provisionar las 2 máquinas que actuarán como servidor en el proveedor que elijas.
- Es preciso la utilización de **tfvars** para poder escalar el número de máquinas desde el pipeline en caso de ser necesario.
- Ejecutar la instalación de nginx desde el propio pipeline. Puedes utilizar bash, Ansible, Fabric, Puppet, Chef... cualquier herramienta con la que te sientas cómodo.
- Debe quedar demostrado que nginx hace balanceo de carga entre las máquinas que actúan de servidor ante un número de peticiones concurrentes.
- La máquina cliente deberá poder comunicarse con el servidor vía IP local (con cURL, por ejemplo) y obtener una página HTML servida desde un fichero index.php.

## Requisitos alternativos (¡¡Bonus!!):
- Utilizar **modules** en el pipeline de Terraform.
- Utilizar **Docker** para ejecutar nginx dentro de un container (extra kudos!).
- Explicar los problemas, pros y contras, que te has encontrado en el proceso de creación del pipeline.
