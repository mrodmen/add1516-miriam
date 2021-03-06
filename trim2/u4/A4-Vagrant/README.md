#1. Introducción

En esta práctica vamos a trabajar con Vagrant, es una herramienta para la creación y configuración de entornos de desarrollo virtualizados.

#2. Primeros pasos

##2.1. Instalar

Para instalar vagrant hay varias formas, nosotros vamos a usar un paquete preparado para descargar del servidor Leela.

##2.2. Proyecto

Creamos un directorio para nuestro proyecto vagrant y ejecutamos el siguiente comando para iniciar la configuración de la máquina:

![imagen](images/1.png)

##2.3. Imagen, caja o box

Ahora necesitamos obtener una imagen de un sistema operativo, hemos descargado la nuestra desde la dirección siguiente: 'vagrant box add micajaXX_ubuntu_precise32 http://files.vagrantup.com/precise32.box'.
Podemos listar las cajas disponibles en nuestro sistema con 'vagrant box list'. Para poder usar una caja determinada en nuestro proyecto, modificamos el fichero 'Vagrantfile' de la carpeta del proyecto.

![imagen](images/2.png)

##2.4. Iniciar la máquina

Vamos a iniciar la máquina virtual creada con Vagrant:

![imagen](images/3.png)

Ejecutando 'vagrant ssh' podemos conectarnos con nuestra máquina virtual:

![imagen](images/4.png)

Comprobamos en VirtualBox como la máquina está creada y funcionando:

![imagen](images/5.png)

Una vez creada la máquina y funcionando se crea el siguiente directorio:

![imagen](images/6.png)

#3. Configuración

##3.1. Carpetas sincronizadas

La carpeta del proyecto que contiene el Vagrantfile comparte los archivos entre el sistema anfitrión y el virtualizado, esto nos permite compartir archivos fácilmente entre los ambientes. Cualquier archivo que coloquemos en este directorio será accesible desde cualquiera de los 2 extremos.

![imagen](images/66.png)

##3.2. Redireccionamiento de los puertos

Uno de los casos más comunes cuando tenemos una máquina virtual es la situación que estamos trabajando con proyectos enfocados a la web, y para acceder a las páginas no es lo más cómodo tener que meternos por terminal al ambiente virtual y llamarlas desde ahí, aquí entra en juego el enrutamiento de puertos.

Modificamos el fichero Vagrantfile, de modo que el puerto 4567 del sistema anfitrión será enrutado al puerto 80 del ambiente virtualizado.

**Más adelante comprobaremos que el redireccionamiento funciona, cuando tengamos instalado el servidor local.**

![imagen](images/7.png)

#4. Suministro
 
##4.1. Suministro mediante script

Suministraremos al ambiente virtual con un pequeño script que instale Apache. Creamos el script 'install_apache.sh', dentro del proyecto:

![imagen](images/8.png)

Modificamos el 'Vagrantfile' y agregamos la siguiente línea de configuración:

![imagen](images/9.png)

Reiniciamos la máquina si ya está en ejecución 'vagrant reload':

Vemos que no nos ha ejecutado ningún cambio, en las dos últimas líneas de código que se ven, en color blanco, nos indica que si no se han producido los cambios en la configuraicón deberemos ejecutar 'vagrant provision':

![imagen](images/10.png)

Ejecutamos 'vagrant provision' y observamos como se producen los cambios: apt-get update, ...

![imagen](images/11.png)

Comprobamos que funciona introduciendo la IP en un navegador de nuestra máquina:

![imagen](images/12.png)

##4.2. Suministro mediante Puppet

Modificamos el fichero 'VagrantFile' y añadimos las siguientes líneas para que use Puppet. Concretamente que utilize el fichero específicado:

![imagen](images/13.png)

Ahora muestro el fichero de configuración de puppet:

![imagen](images/14.png)

Comprobamos que se ejecuta la acción del puppet cuando reiniciamos la máquina en vagrant:

![imagen](images/15.png)

#5. Otras cajas, nuestras cajas

Mi idea principal es la de crear una caja trabajando con Opensuse, empiezo creando el usuario y entrando con él para crear las claves y dar permisos:

![imagen](images/micaja1.png)

Debemos cambiar en el fichero 'sudoers' y añadir la siguiente línea:

![imagen](images/micaja2.png)

Comprobamos que la guest additions están instaladas para mayor comodidad:

![imagen](images/micaja3.png)

Una vez realizadas estas configuraciones deberíamos poder ser capaces de crar el paquete vagrant de dicha máquina. Como se puede ver a continuación, he trabajado con 'Opensuse 13.2 puppet client1', dicha máquina aparece como no existente. **Al ver que no pillaba de ninguna manera el nombre de la máquina, intenté volver a realizar toda la configuración, cambie el nombre, hostname, usuario,... Probando con el nombre de la máquina de puppet master si lo pilla, en esta máquina solo cree el usuario, nunca intercambié claves.**

![imagen](images/micaja4.png)

Añadimos la caja:

![imagen](images/micaja5.png)

Cambiamos en el fichero VagrantFile:

![imagen](images/micaja6.png)

Levantamos la máquina, aquí nos da error en la conexioń SSH:

![imagen](images/micaja7.png)

La máquina ya está creada y funcionando, el problema principal es que no puedo acceder como usuario 'Vagrant':

![imagen](images/micaja9.png)

Añadí las dos líneas que puso mi compañero Kevin en el foro a ver si conseguía arreglar algo:

![imagen](images/micaja10.png)

Vuelvo a intentar entrar, me da error en el intercambio de claves (yo no he realizado intercambio en la máquina como ya he dicho anteriormente, ya que trabajé en client1 no en master).

![imagen](images/micaja11.png)


