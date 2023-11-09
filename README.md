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


