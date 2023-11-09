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