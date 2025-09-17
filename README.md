

# Despliegue Automatizado de Super Mario Bros en Azure con Ansible  
**Ingeniería de Software V — Entrega Académica**  
**Luis Manuel Rojas Correa — Código A00399289**

---

## Descripción

Este proyecto automatiza la instalación de Docker y el despliegue de un contenedor con el juego Super Mario Bros en una máquina virtual (VM) en Microsoft Azure, utilizando Ansible como herramienta de configuración.

El objetivo es demostrar la capacidad de gestionar infraestructura remota mediante playbooks, instalando software y ejecutando servicios de forma reproducible y eficiente.

---

## Requisitos

- Ansible instalado en la máquina local.
- Acceso SSH a una VM en Azure con Ubuntu 24.04.
- Puerto 22 abierto en el grupo de seguridad de red (NSG) de Azure.
- Usuario y contraseña de la VM (en este caso: `vmadmin` / `DevOps123!`).

---

## Estructura del Proyecto

```
training-ansible/
├── inventory/
│   └── hosts.ini
├── playbooks/
│   ├── install_docker.yml
│   └── run_container.yml
└── README.md
```

---

## Configuración

### 1. Archivo de inventario: `inventory/hosts.ini`

Contiene la dirección IP pública de la VM y las credenciales de acceso:

```ini
[azure_vm]
4.227.220.214 ansible_user=vmadmin ansible_ssh_pass=DevOps123!
```

> Nota: Reemplaza la IP si usas una VM diferente.

---

### 2. Playbook: `install_docker.yml`

Instala Docker en la VM siguiendo los pasos oficiales:

- Actualiza el sistema.
- Instala dependencias.
- Agrega la clave GPG y el repositorio de Docker.
- Instala `docker-ce`, `docker-ce-cli` y `containerd.io`.
- Inicia y habilita el servicio Docker.
- Añade el usuario al grupo `docker`.

---

### 3. Playbook: `run_container.yml`

Despliega el contenedor del juego Super Mario Bros:

- Instala la librería Python `docker` (requerida por Ansible).
- Ejecuta la imagen `pengbai/docker-supermario:latest`.
- Mapea el puerto 8787 del host al 8080 del contenedor.
- Configura reinicio automático.

---

## Ejecución

Desde la raíz del repositorio, ejecuta en orden:

```bash
ansible-playbook -i inventory/hosts.ini playbooks/install_docker.yml
```

```bash
ansible-playbook -i inventory/hosts.ini playbooks/run_container.yml
```

---

## Acceso al Juego

Para acceder al juego desde el navegador:

1. En Azure Portal, abre el puerto **8787** en el NSG de la VM (TCP, origen: Any).
2. Visita en tu navegador:

```
http://4.227.220.214:8787
```

---

## Verificación

Conéctate por SSH a la VM:

```bash
ssh vmadmin@4.227.220.214
```

Verifica que el contenedor esté activo:

```bash
sudo docker ps
```

Salida esperada:

```
CONTAINER ID   IMAGE                              COMMAND             CREATED       STATUS       PORTS                    NAMES
4e507ce1f54f   pengbai/docker-supermario:latest   "catalina.sh run"   20 min ago    Up 20 min    0.0.0.0:8787->8080/tcp   supermario-container
```

---

## Resultados

- Docker instalado correctamente en la VM remota.
- Contenedor de Super Mario Bros ejecutándose y accesible vía web.
- Playbooks idempotentes: pueden ejecutarse múltiples veces sin errores.
- Infraestructura completamente automatizada y reproducible.

---

## Entrega Académica

- Asignatura: Ingeniería de Software V
- Estudiante: Luis Manuel Rojas Correa
- Código: A00399289
- Repositorio: https://github.com/Lrojas898/training-ansible
- Fecha de entrega: [Colocar fecha actual]

