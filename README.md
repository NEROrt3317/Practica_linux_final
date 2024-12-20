# Practica_linux_final
Esto en la entrega de practica de linux final 
# Estudiantes _NEHIR RODRIGUEZ TAPIAS - SOFIA LOPERA DIEZ & JUAN DAVID IDARRAGA_
# ejercicios 
1. 3 grupos con 3 usuarios cada uno.
2. Cada usuario debe tener mínimo un archivo.txt o .doc
3. Para cada grupo uno de los usuarios debe tener activos todos los permisos a los archivos de los demás usuarios del grupo y, además, los otros usuarios del grupo, solo pueden tener acceso de lectura de los archivos de los otros usuarios
4. En el grupo que llamaremos principal un usuario tendrá todos los permisos a todos los archivos de los demás grupos

# SOLUCION 1 
## crear grupos
```
sudo groupadd gruponehir
sudo groupadd gruposofia
sudo groupadd grupojuancho
```

## Crear los usuarios y asignarlos a los grupos:
### Para gruponehir
```
sudo useradd -m -G gruponehir usuario1
sudo useradd -m -G gruponehir usuario2
sudo useradd -m -G gruponehir usuario3
```
### Para gruposofia
```
sudo useradd -m -G gruposofia usuario4
sudo useradd -m -G gruposofia usuario5
sudo useradd -m -G gruposofia usuario6
```

### Para grupojuancho
```
sudo useradd -m -G grupojuancho usuario7
sudo useradd -m -G grupojuancho usuario8
sudo useradd -m -G grupojuancho usuario9
```

## Crear contraseña para los usuarios
_NOTA_: No funciona para todas las distribuciones de linux, tener en cuenta 

```
echo "contraseña1" | sudo passwd --stdin usuario1
echo "contraseña2" | sudo passwd --stdin usuario2
echo "contraseña3" | sudo passwd --stdin usuario3

echo "contraseña4" | sudo passwd --stdin usuario4
echo "contraseña5" | sudo passwd --stdin usuario5
echo "contraseña6" | sudo passwd --stdin usuario6

echo "contraseña7" | sudo passwd --stdin usuario7
echo "contraseña8" | sudo passwd --stdin usuario8
echo "contraseña9" | sudo passwd --stdin usuario9
```
_NOTA_:
Estos comandos harán lo siguiente:

`groupadd`: crea un nuevo grupo.
`useradd -m -G`: crea un usuario con un directorio de inicio y lo asigna a un grupo.
`passwd`: establece una contraseña para el usuario (puedes configurar las contraseñas de manera manual usando sudo passwd nombre_usuario si --stdin no está disponible).

# SOLUCION 2
# Crear archivos .txt para los usuarios del gruponehir
```
sudo -u usuario1 touch /home/usuario1/archivo1.txt
sudo -u usuario2 touch /home/usuario2/archivo2.txt
sudo -u usuario3 touch /home/usuario3/archivo3.txt
```
# Crear archivos .txt para los usuarios del gruposofia
```
sudo -u usuario4 touch /home/usuario4/archivo1.txt
sudo -u usuario5 touch /home/usuario5/archivo2.txt
sudo -u usuario6 touch /home/usuario6/archivo3.txt
```
# Crear archivos .txt para los usuarios del grupojuancho
```
sudo -u usuario7 touch /home/usuario7/archivo1.txt
sudo -u usuario8 touch /home/usuario8/archivo2.txt
sudo -u usuario9 touch /home/usuario9/archivo3.txt
```

_NOTA_:
`sudo -u nombre_usuario`: Ejecuta el comando como el usuario especificado.
`touch /home/nombre_usuario/archivo.txt`: Crea un archivo de texto vacío en el directorio de inicio del usuario.
Si prefieres que los archivos sean de formato .doc, simplemente cambia la extensión de los archivos
EJEMPLO
```
sudo -u usuario1 touch /home/usuario1/archivo1.doc
```
Consideraciones:

El comando touch no crea automáticamente los directorios; solo crea archivos. Por lo tanto, si los directorios de inicio de los usuarios no existen, tendrás que crearlos antes de ejecutar los comandos touch.

Sin embargo, si los usuarios fueron creados con la opción -m al usar useradd, los directorios de inicio ya deberían existir. En caso de que necesites crearlos manualmente, puedes hacerlo con el comando mkdir:
```
sudo mkdir -p /home/usuario1
sudo mkdir -p /home/usuario2
sudo mkdir -p /home/usuario3
```
Repite esto para los demás usuarios.

# SOLUCION 3 

### Configurar permisos en gruponehir
```
sudo chmod 770 /home/usuario2/archivo2.txt
sudo chmod 770 /home/usuario3/archivo3.txt
sudo chown usuario2:gruponehir /home/usuario2/archivo2.txt
sudo chown usuario3:gruponehir /home/usuario3/archivo3.txt
```
### Dar permisos de lectura al grupo
```
sudo chmod 750 /home/usuario1/archivo1.txt
sudo setfacl -m u:usuario1:rwx /home/usuario2/archivo2.txt
sudo setfacl -m u:usuario1:rwx /home/usuario3/archivo3.txt
```
### Configurar permisos en gruposofia
```
sudo chmod 770 /home/usuario5/archivo2.txt
sudo chmod 770 /home/usuario6/archivo3.txt
sudo chown usuario5:gruposofia /home/usuario5/archivo2.txt
sudo chown usuario6:gruposofia /home/usuario6/archivo3.txt

sudo chmod 750 /home/usuario2/archivo2.txt
sudo setfacl -m u:usuario4:rwx /home/usuario5/archivo2.txt
sudo setfacl -m u:usuario4:rwx /home/usuario6/archivo3.txt
```
### Configurar permisos en grupojuancho
```
sudo chmod 770 /home/usuario8/archivo2.txt
sudo chmod 770 /home/usuario9/archivo3.txt
sudo chown usuario8:grupojuancho /home/usuario8/archivo2.txt
sudo chown usuario9:grupojuancho /home/usuario8/archivo2.txt

sudo chmod 750 /home/usuario3/archivo3.txt
sudo setfacl -m u:usuario7:rwx /home/usuario8/archivo2.txt
sudo setfacl -m u:usuario7:rwx /home/usuario9/archivo3.txt
```
_NOTA_:
Tener en ceunta que se dividen en 3 7-5-0: lo cual me indica 
1. primer intem (3)
Propietario: Tiene lectura, escritura y ejecución (7 = 4+2+1).
2. segundo item (5)
Grupo : Tiene lectura y ejecución (5 = 4+1).
3. item item(0)
Otros: No tienen ningún permiso (0).
`chmod 770`: Permite al propietario y al grupo leer, escribir y ejecutar; otros no tienen ningún permiso.
`chmod 750`: Permite al propietario todos los permisos, al grupo solo lectura y ejecución, y otros no tienen permisos.
`setfacl -m u`:usuario:rwx: Configura permisos de lectura, escritura y ejecución para un usuario específico sobre un archivo.
`chown`: Cambia el propietario y el grupo de un archivo.


Con estos comandos, cada usuario líder de grupo (`usuario1`, `usuario4`, `usuario7`) tendrá acceso total a los archivos de los demás usuarios del grupo, mientras que los otros usuarios solo tendrán permisos de lectura sobre los archivos de los demás.

# SOLUCION 4
En este caso el grupo principal será `gruponehir` y el usuario uno tendrá acceso a todo 

Queremos que `usuario1` tenga acceso total a estos archivos. Recuerde que `usuario1` ya tiene los permisos de `gruponehir`

### Dar permisos completos (lectura, escritura, ejecución) a usuario1 de gruposofia
```
sudo setfacl -m u:usuario1:rwx /home/usuario4/archivo1.txt
sudo setfacl -m u:usuario1:rwx /home/usuario5/archivo2.txt
sudo setfacl -m u:usuario1:rwx /home/usuario6/archivo3.txt
```
1. Para los archivos del grupo gruposofia:
Los archivos de los usuarios del grupo gruposofia son:

* `/home/usuario4/archivo1.txt`
* `/home/usuario5/archivo2.txt`
* `/home/usuario6/archivo3.txt`

### Dar permisos completos (lectura, escritura, ejecución) a usuario1 de grupojuancho
```
sudo setfacl -m u:usuario1:rwx /home/usuario7/archivo1.txt
sudo setfacl -m u:usuario1:rwx /home/usuario8/archivo2.txt
sudo setfacl -m u:usuario1:rwx /home/usuario9/archivo3.txt
```
2. Para los archivos del grupo grupojuancho:
Los archivos de los usuarios del grupo grupojuancho son:

* `/home/usuario7/archivo1.txt`
* `/home/usuario8/archivo2.txt`
* `/home/usuario9/archivo3.txt`

_NOTA_:

`setfacl -m u:usuario1:rwx`: Le da a usuario1 todos los permisos (rwx) sobre el archivo.
`setfacl -m u:usuario2:r` & `setfacl -m u:usuario3:r:` Les da a los usuarios usuario2 y usuario3 solo permisos de lectura (r).
