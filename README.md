Documentación de Proxy Inverso - 1.0.0

Introducción

Este documento describe la configuración de un proxy inverso utilizando dos servidores Debian con Nginx para el dominio example.test.

Configuración del Proxy Inverso

Escenario

Servidor Web (w1): Servirá páginas web y estará en la dirección 192.168.57.11.

Servidor Proxy (proxy): Actuará como proxy inverso en la dirección 192.168.57.10.

Los usuarios accederán a www.example.test, que será gestionado por el proxy y redirigido al servidor web w1.

Configuración de Vagrant

El despliegue de los servidores se realizará con Vagrant mediante el siguiente archivo Vagrantfile:

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"
  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "256"
  end
  
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update && apt-get install -y nginx
  SHELL
  
  config.vm.define "proxy" do |proxy|
    proxy.vm.hostname = "www.example.test"
    proxy.vm.network "private_network", ip: "192.168.57.10"
  end
  
  config.vm.define "web" do |web|
    web.vm.hostname = "w1.example.test"
    web.vm.network "private_network", ip: "192.168.57.11"
  end  
end

Configuración del Servidor Web (w1)

El servidor w1 responderá en el puerto 8080 con la siguiente configuración en /etc/nginx/sites-available/default:

server {
    listen 8080;
    listen [::]:8080;

    server_name w1;
    root /var/www/html;
    index index.html index.htm;
    
    location / {
        try_files $uri $uri/ =404;
    }
}

Página Web de Prueba

Crear el archivo /var/www/html/index.html con el siguiente contenido:

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>example.test</title>
</head>
<body>
    <h1>example.test</h1>
    <h2>Bienvenido</h2>
    <p>Servidor w1</p>
</body>
</html>

Reiniciar Nginx:

sudo systemctl restart nginx

Configuración del Proxy Inverso

Modificación del archivo /etc/hosts

Agregar la siguiente línea en el servidor proxy:

192.168.57.11 w1

Configuración del proxy inverso en /etc/nginx/sites-available/default

server {
    listen 80;
    listen [::]:80;

    server_name example.test www.example.test;

    location / {
        proxy_pass http://192.168.57.11:8080;
    }
}

Reiniciar Nginx:

sudo systemctl restart nginx

Ahora, al acceder a http://www.example.test, la petición será redirigida al servidor w1 en http://w1:8080.

Comprobación

Para probar la configuración, instalar curl en w1 y ejecutar:

sudo apt install -y curl
curl http://localhost:8080

Si todo está configurado correctamente, deberíamos ver el contenido de la página web alojada en w1.

Licencia

Este documento está bajo la licencia MIT.

Autor: Fernando Raya
Generado con: Sphinx y @pradyunsg's Furo

