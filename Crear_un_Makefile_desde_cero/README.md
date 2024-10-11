# ``Cómo construir un archivo Makefile desde cero: ``
#
#
```bash
NAME = Ejecutable

GCC = g++

FLAGS = -Wall -Wextra -Werror -std=c++98

SRCS =  main.cpp archivo_01.cpp archivo_02.cpp archivo_03.cpp

OBJS = $(SRCS:.cpp=.o)

all: $(NAME)

$(NAME): $(OBJS)
	$(GCC) $(FLAGS) $(OBJS) -o $(NAME)

%.o: %.cpp
	$(GCC) $(FLAGS) -c $< -o $@

clean:
	rm -f $(OBJS)

fclean: clean
	rm -f $(NAME)

re: fclean all

.PHONY = all clean fclean re
```

Un archivo `Makefile` es un script que se utiliza para automatizar la compilación de un proyecto en C++. Los `Makefile` permiten definir reglas para compilar y enlazar archivos, facilitando el manejo de proyectos grandes con múltiples archivos fuente. A continuación, te lo explico al detalle:

### 1. **Variables**
   Las variables permiten definir y reutilizar valores a lo largo del `Makefile`.

   - `NAME = Ejecutable`: Esta variable define el nombre del archivo ejecutable que se va a generar. En este caso, el ejecutable se llamará `Ejecutable`.
   - `GCC = g++`: Define el compilador que se va a usar, que en este caso es `g++`, el compilador de C++.
   - `FLAGS = -Wall -Wextra -Werror -std=c++98`: Esta variable almacena las opciones o "flags" del compilador. 
     - `-Wall`: Activa todas las advertencias comunes.
     - `-Wextra`: Activa advertencias adicionales.
     - `-Werror`: Convierte las advertencias en errores (obliga a corregirlas).
     - `-std=c++98`: Define que el código se debe compilar usando el estándar C++98.
   - `SRCS = main.cpp ...`: Aquí defines los archivos fuente (`.cpp`) que forman parte del proyecto.
   - `OBJS = $(SRCS:.cpp=.o)`: Define los archivos objeto (`.o`) que se generarán a partir de los archivos fuente. Esta expresión toma cada archivo `.cpp` en `SRCS` y lo convierte en su equivalente `.o`.

### 2. **Reglas del Makefile**
   Las reglas definen cómo construir los diferentes componentes del proyecto. Cada regla sigue el formato:

   ```
   objetivo: dependencias
       acción
   ```

   #### `all: $(NAME)`
   - Esta es la regla principal, y le indica a `make` que cuando ejecutes el comando `make all`, debe generar el archivo `$(NAME)` (es decir, el ejecutable `Ejecutable`).

   #### `$(NAME): $(OBJS)`
   - Esta regla especifica que para generar el ejecutable `$(NAME)`, necesitas los archivos objeto `$(OBJS)`.
   - La acción asociada es `$(GCC) $(FLAGS) $(OBJS) -o $(NAME)`, lo que significa que el compilador (`g++`) tomará los archivos objeto y los enlazará en un ejecutable llamado `Ejecutable`.

   #### `%.o: %.cpp`
   - Esta regla define cómo compilar cada archivo `.cpp` en su archivo objeto correspondiente (`.o`).
   - `%` es un comodín que coincide con el nombre de cualquier archivo.
   - La acción `$(GCC) $(FLAGS) -c $< -o $@` indica que el compilador debe tomar un archivo `.cpp` (`$<`), compilarlo y generar el archivo `.o` correspondiente (`$@`).

### 3. **Tareas de limpieza**
   Estas son tareas auxiliares para mantener el proyecto limpio y ordenado.

   #### `clean:`
   - Elimina los archivos objeto (`.o`) generados.
   - La acción `rm -f $(OBJS)` elimina esos archivos.

   #### `fclean: clean`
   - Esta tarea extiende la tarea `clean`, y además de eliminar los archivos objeto, elimina el ejecutable (`$(NAME)`).
   - `rm -f $(NAME)` se encarga de eliminar el ejecutable.

   #### `re: fclean all`
   - Esta regla combina la limpieza completa (`fclean`) con una recompilación completa (`all`). Es útil si quieres asegurarte de que todo se recompila desde cero.

### 4. **.PHONY**
   - `.PHONY = all clean fclean re` indica que estas reglas no corresponden a archivos reales del sistema, sino que son simplemente nombres de comandos. Esto evita posibles conflictos si existieran archivos llamados `all`, `clean`, etc.

### Resumen del funcionamiento
1. **Compilación**: Al ejecutar `make`, se compilarán todos los archivos `.cpp` listados en `SRCS` a archivos objeto `.o`. Luego, estos archivos `.o` se enlazan para crear el ejecutable `Ejecutable`.
2. **Limpieza**: Puedes ejecutar `make clean` para eliminar los archivos objeto o `make fclean` para eliminar también el ejecutable.
3. **Recompilación**: `make re` limpia todo y recompila el proyecto desde cero.

El `Makefile` automatiza estas tareas, asegurando que solo se recompilan las partes necesarias del proyecto y facilitando el desarrollo.

## Pero... ¿cómo sabe el Makefile que, cuando el objetivo es: `$(NAME)`; y su dependencia es: `$(OBJS)`; debe compilar primero usando la regla: "%.o: %.cpp"?

Entiendo que esta parte puede ser confusa, ya que no parece obvio cómo el `Makefile` conecta las reglas para compilar archivos fuente en archivos objeto cuando el objetivo principal es el `Ejecutable`. Vamos a desglosar cómo funciona este proceso en detalle.

### Flujo de trabajo de `Makefile`

Cuando ejecutas el comando `make`, el proceso sigue ciertos pasos para cumplir con las dependencias y reglas que están definidas.

#### Paso 1: Evaluación del objetivo principal
El objetivo principal en este caso es: `$(NAME)` (es decir, el archivo `Ejecutable`).

```makefile
$(NAME): $(OBJS)
```

Esto le dice a `make` que para construir `$(NAME)` (el `Ejecutable`), necesita primero los archivos objeto `$(OBJS)`. En este punto, el `Makefile` aún no sabe cómo generar esos archivos objeto, pero sabe que los necesita.

#### Paso 2: Búsqueda de reglas para satisfacer las dependencias
`make` ahora intenta averiguar cómo obtener los archivos objeto, que están definidos en `$(OBJS)`. Recuerda que la variable `$(OBJS)` se definió previamente como:

```makefile
OBJS = $(SRCS:.cpp=.o)
```

Esto significa que `OBJS` contiene la lista de archivos objeto correspondientes a los archivos fuente `.cpp`. Por ejemplo, `main.cpp` se convierte en `main.o`, `archivo_01.cpp` en `archivo_01.o`, y así sucesivamente.

Aquí es donde entra en juego la regla **patrón**:

```makefile
%.o: %.cpp
	$(GCC) $(FLAGS) -c $< -o $@
```

Esta regla le indica a `make` cómo generar un archivo objeto (`%.o`) a partir de un archivo fuente (`%.cpp`). En este caso:
- `%.o` es un comodín que representa cualquier archivo objeto (`main.o`, `archivo_01.o`, etc.).
- `%.cpp` es el archivo fuente correspondiente (`main.cpp`, `archivo_01.cpp`, etc.).
- El comando `$(GCC) $(FLAGS) -c $< -o $@` se usa para compilar cada archivo fuente por separado.

- `$<` es una variable automática en `make` que representa el archivo fuente (por ejemplo, `main.cpp`).
- `$@` es otra variable automática que representa el archivo objetivo (en este caso, el archivo objeto como `main.o`).

Por lo tanto, cuando `make` ve que necesita `main.o` para crear el `Ejecutable`, busca una regla que le explique cómo generar `main.o`. Encuentra la regla patrón `%.o: %.cpp`, que le indica cómo compilar cualquier archivo `.cpp` en su archivo objeto correspondiente.

#### Paso 3: Aplicación de la regla patrón
Por cada archivo fuente en `$(SRCS)`, `make` aplica la regla patrón `%.o: %.cpp`. Esto significa que:

1. **Si `main.o` no existe o `main.cpp` ha cambiado**, `make` ejecutará el comando:
   ```bash
   g++ -Wall -Wextra -Werror -std=c++98 -c main.cpp -o main.o
   ```
2. **Si `file_01.o` no existe o `file_01.cpp` ha cambiado**, `make` ejecutará:
   ```bash
   g++ -Wall -Wextra -Werror -std=c++98 -c archivo_01.cpp -o archivo_01.o
   ```

Y así sucesivamente, hasta generar todos los archivos objeto necesarios.

#### Paso 4: Construcción del `Ejecutable`
Una vez que todos los archivos objeto (`$(OBJS)`) han sido generados correctamente, `make` vuelve a la regla principal:

```makefile
$(NAME): $(OBJS)
	$(GCC) $(FLAGS) $(OBJS) -o $(NAME)
```

Ahora que los archivos objeto (`main.o`, `archivo_01.o`, etc.) están listos, `make` ejecuta el comando:

```bash
g++ -Wall -Wextra -Werror -std=c++98 main.o archivo_01.o ... -o Ejecutable
```

Esto enlaza los archivos objeto y crea el `Ejecutable`.

### Resumen:
- El objetivo principal es `$(NAME)` (el `Ejecutable`), pero su dependencia son los archivos objeto `$(OBJS)`.
- `make` busca una regla que explique cómo generar los archivos objeto a partir de los archivos fuente.
- La regla patrón `%.o: %.cpp` le dice a `make` cómo compilar cualquier archivo `.cpp` en su archivo objeto correspondiente `.o`.
- Una vez que se han generado todos los archivos objeto, `make` enlaza estos archivos para crear el ejecutable.

El `Makefile` sabe qué hacer porque sigue las dependencias y reglas de manera lógica: empieza con el objetivo principal, encuentra sus dependencias, busca cómo satisfacer esas dependencias (usando reglas como la de patrón), y finalmente ejecuta los comandos necesarios para compilar y enlazar los archivos.