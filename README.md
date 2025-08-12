# TALLER | SERVIDORES LINUX 2025

Este proyecto busca utilizar dos playbook de Ansible en dos servidores linux. En una distribución de Centos 9 se busca instalar y configurar un servidor NFS y en una distribución Ubuntu 24.04 se busca hardenizar el servidor. 

## Requerimientos

- Es necesario contar con tres servidores linux, un centos, un ubuntu y servidor de bastión que podría ser cualquier distribución.
- El bastión debe poder conectarse con los servidores Centos y Ubuntu
- Para ejecutar los playbook tal y como estan representados en este documento es necesario que exista el mismo usuario en todos los servidores (e idealmente con la misma contraseña).

## Preparación

Para poder clonar el repositorio y ejecutar los playbook es necesario contar con Git y Ansible instalado en el servidor bastión.

#### Instalar git
```bash
sudo dnf install git -y
```

#### Instalar Ansible
```bash
sudo dnf install ansible-core -y
```

Una vez con los paquetes instalados, copiamos el repositorio a nuestro home y nos pocisionamos ahí
### ⚠️ 🚨  DE AHORA EN ADELANTE TODOS LOS COMANDOS QUE EJECUTEMOS SE REALIZARAN DESDE ESTA RUTA  🚨 ⚠️ 
```bash
git clone https://github.com/bruno2712/OBLIGATORIO-TSL.git
cd OBLIGATORIO-TSL
```
Primero debemos editar el archivo de inventario y debemos reemplazar las IP que allí aparecen por las de nuestros servidores

```bash
vim ./inventory.inventory.ini

[Centos]
tsl-centos	ansible_host=192.168.2.51

[Ubuntu]
tsl-ubuntu	ansible_host=192.168.2.50

[Linux:children]
Centos
Ubuntu

[Fileserver]
tsl-centos
```
Luego para instalar los modulos necesarios para ejecutar los playbook ejecutamos el siguiente comando

```bash
sudo ansible-galaxy collection install -r ./collections/requirements.yaml
``` 

Por ultimo, vamos a generar una key de autenticación (si ya tenemos una no hace falta generarla) y la vamos a copiar en los servidores Centos y Ubuntu

 ```bash
ssh-keygen
ssh-copy-id [ IP DEL SERVIDOR ]
``` 

