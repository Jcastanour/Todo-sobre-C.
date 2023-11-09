# Todos los codigos de C

## Notacion basica de c

```c 
#include <stdio.h>
Proporciona printf y scanf
```

```c
scanf("formato de entrada", 
&variable1, &variable2, ...);

printf("formato de salida", 
variable1, variable2, ...);
```

Para cadena de textos es muy util usar
`ej. char nombre[50];`

**Tabla con formatos de entrada/ salida**
| Formato | Descripción             |
|---------|-------------------------|
| `%d`    | Entero                  |
| `%f`    | Número de punto flotante|
| `%c`    | Carácter                |
| `%s`    | Cadena de caracteres    |
| `%p`    | Puntero                 |
| `%x`    | Número hexadecimal      |
| `%s`    | Cadena de texto         |

**Apuntador:** es una variable que almacena la direccion de memoria de una variable

`<tipo de dato de la variable al cual apunta> * <nombre del apuntador>;`

**Referencia:** `&` se le pasa la variable a la que quieres apuntar

### Compilacion

- La mayoria de sistemas linux/unix (mac es basado en unix) ya traen compilador para C/C++. Generamente GNU GCC
- Windows es necesario instalar un compilador (Ej. MinGw, cygwin)


**Los archivos se deben guardar con la extencion .c**
La compilacion se hace a traves del comando:
```bash
gcc -o ejecutable archivo.c
```
Se crea un ejecutable que se ejecuta con:
`./ejecutable`


## Notacion basica de linux

**Listar procesos en linux**
| Campo | Descripción                                                |
|-------|------------------------------------------------------------|
| PID   | Identificador del proceso (Process ID)                     |
| TTY   | Dispositivo terminal en el que el proceso está ejecutando  |
| TIME  | Tiempo total de CPU utilizado por el proceso               |
| CMD   | Comando ejecutado por el proceso con argumentos/parámetros |

### Ejemplos de Uso:

1. `ps x`: Muestra todos los procesos en ejecución.

2. `ps ax`: Muestra todos los procesos en el sistema, no solo los del usuario.

3. `ps u`: Incluye más información detallada en el listado de procesos.

4. `ps w`: Muestra la línea de comandos completa (columna CMD), no solo la que cabe en una línea.

Importante saber que se puede usar: `#man command` para ver el manual de ese comando

Tambien existe `#command -- help` da una ayuda o soporte sobre el comando.

### Monitorear los procesos

Usamos el comando `top`, para monitorear en tiempo real los procesos, muy similar a ps, pero en tiempo real.

### Instalacion en linux

Aveces se necesitara instalar herramientas por consola, esto lo hacemos con:
Ejemplo

#### Herramientas oficiales
```c
yum group install "Development Tools"
```

#### Herramientas no oficiales

Se descarga un archivo comprimido .tar.gz

! Util usar herramientas como:
* Putty: para conectarse de forma remota.
* winscp: conectarse a los archivos de forma remota.

Descomprimir:
```c
tar -zxvf <nombre_archivo.tar.gz>
```

y luego se ejecutan:
```bash
./configure
make
make install
```

**comando size:** `size nombre_archivo_ejecutable`

| text   | data   | bss  | dec   | hex  | filename                        |
|--------|--------|------|-------|------|---------------------------------|
| 12276  | 2044   | 1072 | 15492 | 3cc4 | nombre_archivo_ejecutable       |

- **text:** Tamaño de la sección de código ejecutable.
- **data:** Tamaño de la sección de datos (variables inicializadas).
- **bss:** Tamaño de la sección BSS (bloque sin inicializar, para variables no inicializadas).
- **dec:** Suma total de text, data y bss.
- **hex:** Representación hexadecimal de dec.
- **filename:** Nombre del archivo ejecutable.

Se puede detener el proceso con un Ctl + c, si este no se detiene puedes hacerlo desde una consola de comando con:

**comando kill:** `kill -9 <pid>` "Mata" todo el proceso y lo detiene del todo.

## Sistemas operativos en c

### Procesos padres y procesos hijos
Se debe incluir al incio `#include <unistd.h>` Este trae consigo, el fork, exec, sleep.

Un archivo creado en .c sin ejecutar es una entidad estatica 

Cuando se ejectua pasa a ser una entidad dinamica y es lo que se conoce como

**Procesos:** programa en ejecucion

**fork:** Crea un nuevo proceso (duplica el existente). A este nuevo proceso se le llama proceso hijo.

**USO DE FORK**

```c
int rc = fork(); Esta linea duplica el proceso.
if (rc < 0){
    //Aca se ejecuta lo que tenga que hacer en caso de fallar 
exit(1);
} else if (rc == 0) 
{
    //Aca se ejecuta todo lo que queremos que haga el nuevo proceso. conocido como proceso hijo.
} else {
    // Aca se ejecuta todo lo del proceso padre, el existente
}
```

**Execlp:** Sobreescribe la imagen del proceso, la reemplaza por lo que se le diga

**USO DE FORK**

```c
int rc = fork(); //Duplicamos el proceso, por lo que tenemos en este caso 2 procesos
if (rc < 0){
    //Aca se ejecuta lo que tenga que hacer en caso de fallar 
exit(1);
} else if (rc == 0) 
{
    //Aca se ejecuta todo lo que queremos que haga el nuevo proceso. conocido como proceso hijo. Supongamos que queremos que haga otro proceso existente
    excec("ls") //proceso ls lista los directorios.
} else {
    // Aca se ejecuta todo lo del proceso padre, el existente
}
```

### Hilos

Se debe incluir al inicio `#include <pthread.h>`
Estos se compilan con:
`gcc -pthread -o <archivo_ejecutable> <archivo_fuente>.c`

Un proceso tiene unas tareas indivduales que se puede realizar de forma independientes, esto son los **hilos**.

Ejemplo. Programa que realiza dos tareas: la primera tarea imprime números pares y la segunda tarea imprime números impares.

- Proceso: Sería tu programa completo, que incluye ambas tareas.
- Hilos: Serían las unidades más pequeñas de ejecución dentro de ese programa. Un hilo estaría encargado de imprimir números pares, y el otro de imprimir números impares.

**USO DE HILOS**

**En C**
```c
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

int main(int argc, char *argv[]){
  pthread_t h1, h2; //Aca solo se va a decir que esas variables van a ser tipo hilo

  pthread_create(&h1, NULL, funcion, NULL); //Se crea el hilo y se ejecuta, no necesariamente en ese orden
  pthread_create(&h2, NULL, funcion, NULL); //Se crea el hilo y se ejecuta, no necesariamente en ese orden

  printf("main() sigue su ejecucion\n");
  printf("main() sigue su ejecucion\n");
  printf("main() sigue su ejecucion\n");

  pthread_join(h1, NULL); //Bloquea al proceso principal y lo pone a esperar hasta que este hilo finalice
  pthread_join(h2, NULL); //Bloquea al proceso principal y lo pone a esperar hasta que este hilo finalice
  return 0;
}
```

**Ejemplo en python**

```py
from threading import Thread

def funcion():
  print("Soy un hilo ident = ",get_ident())
  return

t1 = Thread(target=funcion) #se crea
t2 = Thread(target = funcon) #se crea
t1.start() #se inicia
t2.start() #se inicia

print("hilo principal sigue su ejecucion")
print("hilo principal sigue su ejecucion")
print("hilo principal sigue su ejecucion")

t1.join() #se pone a esperar al principal a que termine
t2.join() #se pone a esperar al principal a que termine

print("Termina principal")
```

Los hilos se ejecutan pero no tienen un orden definido, ellos pasan a una cola de listos, y ahi se ejecutan segun sea lo mejor y determine el sistema operativo.
Para definirle un orden debemos de decirlo explicitamente.

**Uso de funciones**

**Slep:** Sleep(tiempo de segundos que el hilo debe dormir) --> Usado para realizar retrasos controlados, por si necesitamos que un hilo espere antes de hacer ciertas acciones.








