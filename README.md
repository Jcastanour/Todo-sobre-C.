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

**Wait:** Se usa el wait generalmente en el proceso padre, este es bloqueante, por lo que bloquea al proceso padre hasta que el hijo no termine (cuidado con quedarse en un estado de bloqueado, y que el hijo nunca termine)


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

### IPC Y SINCRONIZACION DE PROCESOS

Se suele usar los terminos de productor hace algo y luego escribe(write) y consumidor lee (read) y con eso que le pasaron hace lo suyo.

Estos son modelos de comunicacion, comunicar procesos/hilos.


#### Paso de mensajes

##### Paso de mensajes nombrado
Se debe especificar explicitamente el nombre del proceso con el que se le va a mandar

- `send(Proceso p, message)`
- `receive(Proceso q, message)`

##### Paso de mensajes indirecto
A traves de MQ (colas de mensajes) o puertos (sockets)

- send(MQ A, message)
- receive(MQ A, message)

#### Tuberias (Pasos de mensajes)
- Escribe en un lado de la tuberia y el otro lado lo pone a disposicion para leer.
- un proceso le ingresa datos por un lado y el otro proceso los saca por el otro lado.
- unidreccional.

##### Tuberias sin nombre
Solo se pueden usar si fueron heredadas de un fork

- Fildes es un descriptor, [0] leer en la tuberia y [1] para escribir en la tuberia.

```c
int pipe(int fildes[2]) //fildes[2] --> fildes[0] y fildes[1]
```

##### Tuberias con nombre
Puede acceder cualquier proceso de la maquina

Para crear tuberia:
```c
fd = int mkfifo(const char *nombre_tuberia, mode_t,mode) //Esto devuelve un descriptor fd
```

**Mode:** Ejemplo. 0666 (permisos de write-read)

Para abrir la tuberia:
```c
int open(const char *nombre_tuberia,int flags)
```

Leer una tuberia
```c
ssize_t read(int fd, void *buf, size_t count);
```

- `ssize_t` es un tipo de datos usado para representar tamaño de bloque de datos
- count es el numero de bytes que se van a leer
- buf es un area donde se almacenan los datos, en este caso el buffer

Leer una tuberia
```c
ssize_t write(int fd, const void *buf, size_t count);
```

- const indica que no se debe modificar, en este caso que no se debe modificar los datos a los que apunta buf.

Para cerrar la tuberia:
```c
int close(int fd)
```

**FLAGS**
| Flag            | Descripción                                               |
|-----------------|-----------------------------------------------------------|
| `O_RDONLY`      | Abrir el archivo en modo solo lectura                     |
| `O_WRONLY`      | Abrir el archivo en modo solo escritura                   |
| `O_RDWR`        | Abrir el archivo en modo lectura/escritura                |
| `O_CREAT`       | Si el archivo no existe, créalo                            |
| `O_TRUNC`       | Trunca el archivo a longitud cero al abrirlo              |
| `O_APPEND`      | Abre el archivo para escritura al final                   |
| `O_EXCL`        | Si se utiliza con `O_CREAT`, falla si el archivo ya existe |


**EJEMPLOS DE WRITE Y READ**

**Caso de write:**

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int main(int argc, char *args[]){
  const char ptuberia[] = "/tmp/ptuberia";

  if (mkfifo(ptuberia, 0666)<0){
    perror("error al crear mkfifo");
    return 1;
  }

  int fd = open(ptuberia,O_WRONLY);

  int dato_entra = 200;
  
  write(fd,&dato_entra,sizeof(int));
  printf("Escribiendo %d\n",dato_entra);
  close(fd);
  return 0;
}
```

**Caso de read:**
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int main(int argc, char *args[]){
  const char ptuberia[] = "/tmp/ptuberia";
  int fd = open(ptuberia,O_RDONLY);
  int dato_sale;

  read(fd,&dato_sale,sizeof(int));
  printf("Dato que sale de tuberia %d\n",dato_sale);

  close(fd);
  return 0;
}
```







