# Tabla de Contenido

- [Tabla de Contenido](#tabla-de-contenido)
- [Todos los codigos de C](#todos-los-codigos-de-c)
  - [Notacion basica de c](#notacion-basica-de-c)
    - [Compilacion](#compilacion)
  - [Notacion basica de linux](#notacion-basica-de-linux)
    - [Ejemplos de Uso:](#ejemplos-de-uso)
    - [Monitorear los procesos](#monitorear-los-procesos)
    - [Instalacion en linux](#instalacion-en-linux)
      - [Herramientas oficiales](#herramientas-oficiales)
      - [Herramientas no oficiales](#herramientas-no-oficiales)
  - [Sistemas operativos en c](#sistemas-operativos-en-c)
    - [Procesos padres y procesos hijos](#procesos-padres-y-procesos-hijos)
    - [Hilos](#hilos)
    - [IPC Y SINCRONIZACION DE PROCESOS](#ipc-y-sincronizacion-de-procesos)
      - [Paso de mensajes](#paso-de-mensajes)
        - [Paso de mensajes nombrado](#paso-de-mensajes-nombrado)
        - [Paso de mensajes indirecto](#paso-de-mensajes-indirecto)
      - [Tuberias (Pasos de mensajes)](#tuberias-pasos-de-mensajes)
        - [Tuberias sin nombre](#tuberias-sin-nombre)
        - [Tuberias con nombre](#tuberias-con-nombre)
    - [Memoria compartida en linux con POSIX](#memoria-compartida-en-linux-con-posix)
      - [Crear / abrir memoria compartida](#crear--abrir-memoria-compartida)
      - [Procesos que quieran engancharse](#procesos-que-quieran-engancharse)
    - [Condiciones de carrera](#condiciones-de-carrera)
      - [Soluciones](#soluciones)
        - [Solucion Peterson](#solucion-peterson)
    - [Semaforos](#semaforos)
      - [Semaforos con nombre](#semaforos-con-nombre)
    - [Mutex](#mutex)
      - [Variables condicionales](#variables-condicionales)
    - [EJEMPLO CON USO DE SEMAFOROS, USO DE FORK Y MEMORIA COMPARTIDA.](#ejemplo-con-uso-de-semaforos-uso-de-fork-y-memoria-compartida)


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
| `MAP_SHARED`    | Compartido entre multiples procesos                         |


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

### Memoria compartida en linux con POSIX

Se deben compilar con:
`# gcc -o <archivo_resultado> <archivo_fuente.c> -lrt`

Se implementa mediante archivos mapeados en memoria.

Es un archivo que corresponde a una region de memoria, donde los procesos se enganchan y leen o escriben.

- Posix es un estandar

#### Crear / abrir memoria compartida

Se crea con:
```c
fd = shm_open(name, O_CREAT | O_RDWR, 0666);
```

Se debe establecer un tamaño con:
```c
ftruncate(fd, int size)
```

Se debe ahora enganchar / mapear a esa area de memoria compartida
```c
ptr = mmap(0, SIZE, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
```

Se puede escribir con:
```c
cons char *texto;
sprintf(ptr,"%s",texto);

//Se debe actualizar el apuntador para que ya no apunte al comienzo

ptr += strlen(texto);
```

#### Procesos que quieran engancharse

La deben abrir:
```c

int fd; // descriptor de archivo del área de memoria compartida 
fd = shm_open(name, flag, 0666);
```

Mapea el area de memoria:
```c
char *ptr; // puntero apuntando al area de memoria
ptr = mmap(0, SIZE, PROT_READ, MAP_SHARED, fd, 0);
//direccion de inicio, tamaño, permisos, flag, descriptor, desplazamiento//
```

leer los bytes de la area compartida:
```c
printf("%s\n",(char *)ptr); // (char *)ptr me da el valor que hay en esa direccion de memoria
```

Si ya no se va a usar se desvincula / desengancha:
```c
shm_unlink(name);
```

### Condiciones de carrera

2 o mas procesos escribiendo en el mismo lado

Habian problemas si 2 procesos entran a la vez, a quien se le da prioridad (decide el planificador) y se puede volver un caos (seccion critica).

**Interbloqueos:** Ambos se quedan esperando a que el otro le de paso
**Livelock:** Ej. Cuando 2 personas se mueven para el mismo lado (P_1 manda intencion de entrar, y P_2 tambien lo hace).

#### Soluciones
- Avisar que entro y que salio.
- Exclusion mutua: regla que no pueden haber 2 procesos dentro a la vez.

##### Solucion Peterson
Esta solo es para 2 procesos

sleep() --> Duerma mientras estoy adentro de la region critica.
wakeup(PID) --> Levantese que ya sali de la region critica.

### Semaforos
Permite coordinacion entre procesos o hilos de un programa y evitar condiciones de carrera

Se compila con:
`gcc -pthread -o <archivo_resultado> <archivo_fuente.c> -lrt`

se debe incluir `#include <semaphore.h>`
#### Semaforos con nombre

Crear / abrir:
```c
sem_t *sem_open(const char *name,
                int oflag,
                mode_t mode,
                unsigned int value);
```

- se deberia guardar en una variable `sem_t *sem`, sem guardaria un descriptor que es necesario y usado.

- `sem_t` tipo de datos para representar semaforos
- `value` valor con el que va a iniciar el semaforo

**OFLAGS**
| Oflags      | Descripción                                                         |
|-------------|---------------------------------------------------------------------|
| `O_CREAT`   | Crea el semáforo si no existe.                                      |
| `O_EXCL`    | Si `O_CREAT` también está presente y el semáforo ya existe, la llamada fallará. |
| `O_RDWR`    | Permite tanto lectura como escritura.                               |
| `O_RDONLY`  | Permite únicamente lectura.                                         |
| `O_WRONLY`  | Permite únicamente escritura.                                       |

Se suele tener un patron tipico para proteger la region critica

```c
/* obtener el semáforo */
sem_wait(sem); 
/* sección crítica */
/* liberar el semáforo: signal() */
sem_post(sem);
```

- `sem_wait(sem)` se usa para avisar que entro.
- `sem_post(sem)` se usa para avisar que salio.

### Mutex
Es un objeto que se usa para sincronizar hilos en linux y proteger la region critica.

- Es una variable de tipo `pthread_mutex_t`
- se debe incluir `#include <pthread.h>`

Se crea:
```c
int pthread_mutex_init(pthread_mutex_t *mutex,
                        const pthread_mutexattr_t *attr);
```

- `attr:` apuntador a una estructura iniciliazada desde antes con los parametros deseasdos del mutex

Para destruirlo:
```c
int pthread_mutex_destroy(pthread_mutex_t *mutex);
```

bloquear mutex para proteger:
```c
int pthread_mutex_lock(pthread_mutex_t *mutex);
```

**Patron basico de uso**
```c
pthread_mutex_lock(&mimutex);
/* región crítica */
pthread_mutex_unlock(&mimutex);
/* región no crítica */
```

#### Variables condicionales
Los mutex que se encarguen de garantizar la exclusion mutua y la variable condicional de señalizar a los demas hilos

Se crea con:
```c
int pthread_cond_init(pthread_cond_t *cond,
                        const pthread_condattr_t *attr);
```

Para avisar que hubo un cambio:
```c
int pthread_cond_signal(pthread_cond_t *cond); // para avisar a 1 solo
int pthread_cond_broadcast(pthread_cond_t *cond); // para avisarle a todos
```

Para quedarse esperando a que haya un cambio:
```c
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex);
```

- Toca indicar que mutex estamos esperando.

Se destruye con:
```c
int pthread_cond_destroy(pthread_cond_t *cond);
```

### EJEMPLO CON USO DE SEMAFOROS, USO DE FORK Y MEMORIA COMPARTIDA.
En este ejemplo se comunica un proceso padre con un proceso hijo, el hijo le envio una cadena de textos en minuscula y el padre la recibe y la convierte en mayuscula.

```c
// Proceso principal
int main() {
  const char *name = "mparcial";
  const int SIZE = sizeof(char) * 2000;
  int fd;

  // Crear semáforos
  sem_t *sem_padre;
  sem_t *sem_hijo;

  // Inicializar semáforos con nombre
  sem_padre = sem_open("sem_padre", O_CREAT, 0666, 0);
  sem_hijo = sem_open("sem_hijo", O_CREAT, 0666, 0);

  // Se crea la memoria compartida - Identidicador de la memoria fd(file descriptor)
  fd = shm_open(name, O_CREAT | O_RDWR, 0666);
  if (fd == -1) {
    perror("Error en shm_open");
    return -1;
  }
  // Configura el tamaño de la memoria compartida
  ftruncate(fd, SIZE);

  // Se crea la direccion del puntero
  char *ptr;

  // Se crea el proceso hijo
  int pid = fork(); 
  if (pid < 0) {
    perror("Error al crear al hijo");
    return 1;
  }

  // Se inicia la ejecución principal del codigo
  if (pid == 0) {
    // El hijo mapea la memoria compartida usando el puntero ptr(pointer to reference)
    ptr = mmap(0, SIZE, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    if (ptr == MAP_FAILED) {
      perror("Error mmap (hijo)");
      return -1;
    }

    // Ciclo infinito para leer y escribir datos
    while (1) {
      // Semaforo para que el hijo espere a que el padre escriba algo
      sem_wait(sem_hijo); 
      printf("Proceso hijo recibe: %s", (char *)ptr);
      convertirAMayusculas((char *)ptr);
      // Se le da luz verde al padre, para que imprima los caracteres modificados
      sem_post(sem_padre);
    }
  } else {
    // El padre mapea la memoria compartida usando el puntero ptr(pointer to reference)
    ptr = mmap(0, SIZE, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    if (ptr == MAP_FAILED) {
      perror("Error MAP_FAILED");
      return (-1);
    }

    while (1) {
      char texto_ingresar[SIZE];
      printf("Ingrese cadena de texto: ");
      fgets(texto_ingresar, sizeof(texto_ingresar), stdin);
      sprintf(ptr, "%s", texto_ingresar);
      // Le damos luz verde al hijo, para que transforme la cadena de texto
      sem_post(sem_hijo);
      // Esperamos la cadena de texto transformada
      sem_wait(sem_padre);
      // Imprimimos la cadena de texto transformada
      printf("Padre (despues de la conversion del hijo): %s", ((char *)ptr));
      // Borramos los caracteres que habian en la memoria compartida, para asi no tener que actualizar el apuntador ptr
      memset(ptr, 0, SIZE);
    }
  }
  sem_close(sem_padre);
  sem_close(sem_hijo);
  return 0;
}
```




