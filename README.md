# **Proxy Inverso - Fco. Javier Martín Mariscal**

## **Introducción**
Este documento describe la configuración de un proxy inverso utilizando dos servidores Debian con Nginx para el dominio `example.test`.

## **Configuración del Proxy Inverso**

### **Escenario**
- **Servidor Web (`w1`)**: Servirá páginas web y estará en la dirección `192.168.57.11`.
- **Servidor Proxy (`proxy`)**: Actuará como proxy inverso en la dirección `192.168.57.10`.
- Los usuarios accederán a `www.example.test`, que será gestionado por el proxy y redirigido al servidor web `w1`.

## **Configuración de Vagrant**
El despliegue de los servidores se realizará con Vagrant mediante el siguiente archivo `Vagrantfile`:

![image](https://github.com/user-attachments/assets/85fc5f2e-56d5-4b0f-8098-042b8e25c50e)

## **Configuración del Servidor Web (w1)**
El servidor w1 responderá en el puerto 8080 con la siguiente configuración en /etc/nginx/sites-available/default:

![image](https://github.com/user-attachments/assets/6075bb60-6f16-421b-85be-e23401096cd3)

Crear el archivo /var/www/html/index.html con el siguiente contenido:

![image](https://github.com/user-attachments/assets/da80eae0-a299-4c43-99b2-a913aeec5611)

## **Configuración del Proxy Inverso**
Modificación del archivo /etc/hosts
Agregar la siguiente línea en el servidor proxy:

![image](https://github.com/user-attachments/assets/6a1b9724-ea6d-4618-aa5f-b68b07fe2d5e)

Configuración del proxy inverso en /etc/nginx/sites-available/default

![image](https://github.com/user-attachments/assets/17e077ec-5abd-4411-a777-cbff38157728)

Probamos la configuración:

![image](https://github.com/user-attachments/assets/7092d844-e19c-4e14-94df-b38e5da4ef9a)

![image](https://github.com/user-attachments/assets/56e24e42-553f-4133-b94f-e97da6c0dea6)

## **Comprobaciones:**
Accederemos en nuestro navegador a la dirección 192.168.57.10.

![image](https://github.com/user-attachments/assets/96cd9dba-f0e4-44b7-9fac-86b137f14f97)

Accederemos a los logs de proxy

![image](https://github.com/user-attachments/assets/86f98b39-229e-4d18-b1b8-8d598f5bfa07)

## **Cabeceras**
En Firefox pulsaremos Ctrl+Shift+I u opciones / more tools / Web developer tools. Luego pulsaremos en Network y marcaremos Disable cache:

![image](https://github.com/user-attachments/assets/981985e4-5d1e-4b99-8e0c-f643b4506db1)

Si accedemos mediante un nombre de host www.192.168.57.10.nip.io, veremos que la cabecera Host cambia. Pruebalo

![image](https://github.com/user-attachments/assets/baf06673-81e7-4f26-a236-69acc7b3ff65)

## **Añadiendo cabeceras**
Para añadir cabeceras, en el archivo de configuración del sitio web de proxy debemos añadir dentro del bloque location / { … } debemos añadir la directiva:

![image](https://github.com/user-attachments/assets/d90a2aad-7737-48f0-bbcc-6f3d20dc8f89)

Con las herramientas de desarrollador comprobamos que la petición ha pasado por el proxy inverso que ha añadido la cabecera en la respuesta. Pruébalo y muéstralo (recuerda poner tu nombre).

![image](https://github.com/user-attachments/assets/a0e5128f-f63c-4a27-ac0f-a54ada1859a3)

Añadiremos también una cabecera en el servidor web:

![image](https://github.com/user-attachments/assets/975af503-97d0-4406-9d39-3db545145bf0)

Comprobamos:

![image](https://github.com/user-attachments/assets/2694f5e9-de3a-4800-a656-43061c57dca0)










