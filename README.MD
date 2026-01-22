## LFS PKGBUILD SCRIPTS
Esta es una colección de scripts PKGBUILD que he creado hasta ahora para LFS. Esto cubre todos los paquetes del capítulo 6 (excepto GRUB, que no uso) más paquetes adicionales de BLFS y otros lugares a medida que los creo.

## NOTAS
Antes de poder utilizar estos scripts PKGBUILD, por supuesto necesitarás descargar, compilar e instalar pacman. La mayoría de las dependencias de pacman tienen instrucciones de compilación en BLFS, excepto fakeroot, pero ese es un paquete sencillo.

Yo sugerir inicialmente la instalación de pacman y su dependencias en el directorio /tools y entonces seguir el libro adelante de allí. una vez usted acabado configurar tu sistema, paquete y instalar pacman y su dependencias así que lo será disponible de tu lfs construir una vez usted reiniciar eso.

## ADVERTENCIA

Básicamente, cualquiera de los paquetes del libro que hacen referencia al directorio /tools deberá modificarse para poder utilizarse. Algunos otros paquetes también tienen optimizaciones específicas para mi sistema que quizás no desees..

## LICENCIA 
La mayor parte del código utilizado en los PKGBUILD se toma del libro LFS, cuyo código está bajo una licencia MIT. El aviso de licencia y derechos de autor se encuentra en el archivo LICENCIA en el directorio de nivel superior.


