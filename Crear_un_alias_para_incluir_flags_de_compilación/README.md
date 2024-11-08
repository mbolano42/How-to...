# Crear un alias para incluir flags de compilación

Para crear un alias en tu shell de Linux que incluya automáticamente los flags de compilación cada vez que uses `gcc` (para programas desarrollados en lenguaje C) ó `g++` (para programas desarrollados en lenguaje C++), sigue estos pasos:

1. **Edita el archivo de configuración del shell**.
- Si usas Bash, puedes agregar el alias en el archivo `~/.bashrc` o `~/.bash_aliases`. Abre el archivo con Vim (o cualquier editor de tu preferencia):
   
   ```bash
   vim ~/.bashrc
   ```

2. **Agrega el alias**.
- Añade las siguientes líneas al final del archivo:

   ```bash
   alias gcc='gcc -Wall -Wextra -Werror'
   alias g++='g++ -Wall -Wextra -Werror -std=c++98'
   ```

3. **Aplica los cambios**.
- Guarda el archivo y carga la configuración para que el alias esté disponible en la sesión actual:

   ```bash
   source ~/.bashrc
   ```

A partir de ahora, cada vez que escribas `gcc` ó `g++` en la terminal, se aplicarán automáticamente los flags.