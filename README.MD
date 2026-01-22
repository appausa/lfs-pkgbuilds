## LFS PKGBUILD SCRIPTS
Esta es una colección de scripts PKGBUILD que he creado hasta ahora para LFS. Esto cubre todos los paquetes del capítulo 6 (excepto GRUB, que no uso) más paquetes adicionales de BLFS y otros lugares a medida que los creo.

## NOTAS
Antes de poder utilizar estos scripts PKGBUILD, por supuesto necesitarás descargar, compilar e instalar pacman. La mayoría de las dependencias de pacman tienen instrucciones de compilación en BLFS, excepto fakeroot, pero ese es un paquete sencillo.

Yo sugerir inicialmente la instalación de pacman y su dependencias en el directorio /tools y entonces seguir el libro adelante de allí. una vez usted acabado configurar tu sistema, paquete y instalar pacman y su dependencias así que lo será disponible de tu lfs construir una vez usted reiniciar eso.

## ADVERTENCIA

Básicamente, cualquiera de los paquetes del libro que hacen referencia al directorio /tools deberá modificarse para poder utilizarse. Algunos otros paquetes también tienen optimizaciones específicas para mi sistema que quizás no desees..

## LICENCIA 
La mayor parte del código utilizado en los PKGBUILD se toma del libro LFS, cuyo código está bajo una licencia MIT. El aviso de licencia y derechos de autor se encuentra en el archivo LICENCIA en el directorio de nivel superior.

He aquí una guía para la instalación de Pacman en Linux From Scrach 

Instalación de pacman en su cadena de herramientas temporal

Comenzaremos justo antes del apartado 6.7. Encabezados API de Linux-4.9.9 del libro LFS 12.4.

Dependencias de Pacman 

Pacman depende de los siguientes paquetes:

zlib
libarchive
pkg-config
fakeroot, que a su vez depende de libcap

La mayoría de estos no forman parte del libro LFS, así que descargue sus fuentes manualmente:

wget https://www.libarchive.org/downloads/libarchive-3.3.2.tar.gz
wget http://turul.canonical.com/pool/main/f/fakeroot/fakeroot_1.22.orig.tar.bz2
wget https://sources.archlinux.org/other/pacman/pacman-5.0.2.tar.gz

Compila estos paquetes con el comando siguiente. Al igual que el libro LFS, estos comandos suponen que ha extraído las fuentes relevantes y las ha guardado en el directorio resultante.

zlib 1.2.11

./configure --prefix=/tools

make

make install

libarchive 3.3.2

./configure --prefix=/tools --without-xml2 --disable-shared

make

make install

pkg-config 0.29.2

./configure --prefix=/tools            \
            --with-internal-glib       \
            --disable-compile-warnings \
            --disable-host-tool        \
            --disable-shared           \
            --docdir=/tools/share/doc/

pkg-config-0.29.2

make

make install

libcap 2.25

make

make RAISE_SETFCAP=no lib=lib prefix=/tools install

chmod -v 755 /tools/lib/libcap.so

fakeroot 1.22

./configure --prefix=/tools                 \
            --libdir=/tools/lib/libfakeroot \
            --with-ipc=sysv
make
make install
Pacman 5.0.2
./configure --prefix=/tools   \
            --disable-doc     \
            --disable-shared  \
            --sysconfdir=/etc \
            --localstatedir=/var

make

make install

Esto habrá instalado, entre otros, los archivos de configuración makepkg.conf y pacman.conf en /etc; es posible que desee editarlos. Para makepkg.conf, asegúrese de que CARCH y CHOST sean apropiados, por ejemplo:

CARCH="x86_64"
CHOST="x86_64-pc-linux-gnu"

Puede configurar su nombre y dirección de correo electrónico como PACKAGER si lo desea.

Configurando para construir paquetes con pacman

Cree un nuevo usuario agregando lo siguiente a /etc/passwd:

adalberto:x:1000:999:adalberto:/home/adalberto:/bin/bash

Donde 999 es el ID del grupo de usuarios. Verifique /etc/group para obtener el ID correcto o, si los usuarios no están presentes en ese archivo, agregue lo siguiente.

users:x:999:

Crea el directorio home:

mkdir -v /home/adalberto 

chown -Rv adalberto:adalberto /home/adalberto 

Salga de su chroot actual, luego ingrese a su nuevo usuario (asegúrese de que 1000, 999 y adalberto estén configurados con los valores adecuados para su sistema):

chroot --userspec=1000:999 "$LFS" /tools/bin/env -i \
     HOME=/home/adalberto  \
     TERM="$TERM"          \
     PS1='$? \u:\w \$ '    \
     PATH=/bin:/usr/bin:/sbin:/usr/sbin:/tools/bin:/tools/sbin      \
     /tools/bin/bash --login +h

Es posible que desee crear un directorio de compilaciones en su directorio de inicio. Allí, luego creará un directorio para cada paquete que esté creando.


Instalación de pacman con pacman

copia las fuentes de pacman en su directorio build, ~/build/pacman-5.0.2.

Descargar los archivos necesario para construir los (PKGBUILD, makepkg.conf y pacman.conf.x86_64) desde Arch Linux, paquetes repo a construir en el directorio.

Edits los archivos PKGBUILD:

Comentario fuera del grupos y dependencias de líneas. Ojo no utilice los paquetes de Arch Linux, y porque  lfs es diferente a Arch Linux, pacman no debe tener dependencias de Arch Linux para intalarlo. Editar la fuente variable a Arch, sólo la fuente de pacman tarball (no el url) y el tres descargar archivos se actual. cambio el configurar llamar en el construir que funcionen:

./configure              
    --prefix=/tools      \
    --sysconfdir=/etc    \
    --localstatedir=/var \
    --disable-doc        \
    --disable-shared

comentamos que esta fuera de comprobar función; el pruebas no correr sin Pyton.

Now run the following command as your non-root user from the build directory:
Ahora corre el siguiente comando como el usuario root para construir el directorio:

makepkg --skipchecksums

Puedes saltar la seguridad de comprobación porque nosotros no tener instalado OpenSSL  aún.

si todo va bien, este debería tener creado un archivo que usted puede ahora instalar lo sigue, como root ahora:

pacman -U pacman-5.0.2-2-x86_64.pkg.tar.gz

Instalando paquetes del capítulo seis de el libro LFS ahora al continuar con el descansaremos del libro, porque en vez de intalar paquetes manualmente lo aremos con pacman desde el capítulo seis de el Libro LFS. 

All of the necessary PKGBUILD files can be found in the packages directory, but if you want to learn how to create these, I suggest you only refer to them when you get stuck.

Todo lo que necesitas es los archivos PKGBUILD que se encuentran en el directorio de los paquetes, pero si usted desea aprender cómo a crear estos, yo sugerir que  usted sólo mire los que tengo aquí ya creado si te encuentras enredado.

Creación de Paquetes 

El proceso general va como este:

Crea um nuevo directorio en el directorio builds.

Copia todo los paquetes y parches que necesites (archivos fuentes, y otros posibles  archivos, parches o archivos de configuración).

Escribe las archivos PKGBUILD.

Run makepkg --skipchecksums.

Comprueba el contenido del directorio pkg, que es el que pacman Create, y de esta forma puede verlo.

Instalar el paquete (como root) con pacman -u $ nombre de archivo

Al escribir  o editar el archivo PKGBUILD pode tener errores. Es importante que usted aga una copia del libro LFS, copie y pegue lo que necesita para el archivo PKGBUILD, pero no será fácil esto porque la configuración de PKGBUILD es un poco difernte del libro.

Si estas con dudas, puedes buscar información  en los repo de Arch Linux, en este github.com. 

Tips
Source directory
When calling any of the PKGBUILD functions, makepkg will automatically cd you into the source dir, which is where it will have extracted or symlinked all your source files.

Consejos 
Directorio Fuente  
Donde entrará los paquetes 

Fuente tarball
makepkg donde pacman estrae todos los archivos.

Esto es por qué el primero paso de cada función voluntarias usualmente es cd en el directorio creado durante el extracción de la fuente tarball.

Instalando paquetes
La instalación de los archivos, no se puede instalar en el sistema raíz, pero en su lugar usted intalara en ${pkgdir}. Para cierto paquetes ponerce en destdir = ${pkgdir} instalar en vez de make install. No todo paquetes usan destdir, sin embargo. Si usted está no seguro cómo aquellos paquetes debería ser construido, usted puede siempre comprobar en Arch Linux los paquetes o el PKGBUILDs de este reporte.

En algunos casos, el libro le dera que mueva un directorio. Mientras trabsja con la creación de los paquetes, Pero pude provocar problemas cuando los intale. Debes crear primero un directorio con permiso -vdm755 $dir_name.

Post-install
Pacman ejecuta las funciones he intala los archivos.

6.9. Glibc-2.26
The book wants you to create some symlinks; I did that manually, as I felt it would be out of place in the package.

The tzdata part of glibc requires zic to be installed to the system, which means after glibc was installed with pacman -U. While I suppose you could directly call the zic binary residing in the src or pkg directories, I thought it would be cleaner to split the tzdata installation to a separate package, much like Arch Linux does.

6.10. Adjusting the Toolchain
I did these steps manually. It didn't feel suitable to (ab)use makepkg/pacman for this purpose.

6.15. Bc-1.07.1
The book wants you to create symlinks for libncurses. I did this manually before building the package.

6.20. GCC-7.2.0
Before building the package, increase the stack size: ulimit -s 32768.

I had to use --force when installing this package, since some libraries already existed on the system.

6.28. Shadow-4.5
I ran passwd manually.

6.34. Bash-4.4
When creating the package, makepkg told me that "Package contains reference to $srcdir". Using grep -R "$(pwd)/src" pkg/, I found out that Bash installs Makefile.inc to /usr/lib/bash/, which contains a reference to the build directory. On an existing Arch Linux installation, /usr/lib/bash/Makefile.inc also contained a reference to a (non-existing) build directory, so I assume this is benign.

Use --force to install this package, since /bin/bash was created as part of 6.6. Creating Essential Files and Symlinks.

I then re-chrooted, using /bin/bash instead of /tools/bin/bash.

6.40. Perl-5.26.0
The LFS book tells you to create /etc/hosts. I've chosen to do this manually, rather than to have the Perl package install this file. It doesn't sound right that Perl should own this file. For reference, in Arch Linux, the hosts file is owned by the filesystem package, which contains the base Arch Linux files, so it makes sense that this is created manually in the case of LFS.

Use --force to install this package, since /usr/bin/perl was created as part of 6.6. Creating Essential Files and Symlinks.

6.50. Coreutils-8.27
For some reason, doing the in place sed (sed -i) on chroot.8 resulted in the file having 000 permissions. I replaced it with a regular sed, redirecting the result to a new file, then using install to copy the file to its destination.

Use --force to install this package, since a number of files already exist (amongst others cat, dd, echo) in /bin (as symlinks to /tools). These were added in 6.6. Creating Essential Files and Symlinks.

6.53. Findutils-4.6.0
As with coreutils, using in place sed set the permissions to 000, so I used the same workaround here.

6.63. Sysklogd-1.5.1
Sysklogd's makefile doesn't support specifying a destination directory when installing (make DESTDIR=/path). I've included a patch that adds this.

Stage 4 - Installing pacman to your final system
Stage four starts immediately after the last package in chapter six, currently vim. Before continuing with the rest of the chapter, build and install pacman and its dependencies.

As part of chapter six you should already have installed most of these packages, except for:

libarchive
fakeroot
pacman
Use your temporary pacman installation to install them. Remember, we're installing to the final system, not to /tools.

Stage 5 - Finishing the book
Finish up the rest of the book manually.

When restarting my machine, the LFS system wouldn't boot properly. It turned out that many binaries and other files were owned by ben, the user with which I built the packages. I have no idea why that happened, but chowning them to root:root allowed my system to boot.