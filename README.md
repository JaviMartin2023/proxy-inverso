# **Documentación de Proxy Inverso - 1.0.0**

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



