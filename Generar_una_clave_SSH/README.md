# `Generar una clave SSH.`

Para obtener la clave SSH de tu PC, primero necesitas verificar si ya tienes un par de claves SSH generadas (una `clave pública` y una `clave privada`). Si no tienes una clave SSH generada, tendrás que crear una nueva. Aquí te explico ambos pasos.

### 1. **Verificar si ya tienes una clave SSH:**

Abre una terminal y ejecuta el siguiente comando para comprobar si tienes una clave SSH:

```bash
ls ~/.ssh
```

Si ves los archivos `id_rsa` (clave privada) e `id_rsa.pub` (clave pública), significa que ya tienes un par de claves SSH generadas. La clave pública es la que se suele compartir, por ejemplo, con servicios como GitHub o servidores remotos.

Puedes mostrar el contenido de tu clave pública (la que deberás copiar y pegar cuando te la soliciten) con el siguiente comando:

```bash
cat ~/.ssh/id_rsa.pub
```

Esto mostrará tu clave pública en la terminal. Solo debes copiarla y utilizarla según necesites.

### 2. **Generar una nueva clave SSH (si no tienes una):**

Si no tienes una clave SSH generada o quieres generar una nueva, sigue estos pasos:

1. Ejecuta el siguiente comando en la terminal para generar una nueva clave SSH:

    ```bash
    ssh-keygen -t rsa -b 4096 -C "tu_email@ejemplo.com"
    ```

    - La opción `-t rsa` especifica que usarás el algoritmo RSA.
    - La opción `-b 4096` genera una clave de 4096 bits, que es más segura.
    - La opción `-C` añade un comentario (tu email, en este caso) para identificar la clave.

2. Cuando te pregunte dónde guardar la clave, puedes presionar **Enter** para aceptar la ubicación predeterminada (`~/.ssh/id_rsa`).

3. Después, te pedirá que introduzcas una frase de seguridad (passphrase). Esto es opcional, pero puede añadir una capa adicional de seguridad.

4. Una vez generada, la clave pública estará en `~/.ssh/id_rsa.pub`. Puedes obtenerla con el comando:

    ```bash
    cat ~/.ssh/id_rsa.pub
    ```

### 3. **Agregar tu clave SSH al agente de autenticación:**

Para que tu sistema utilice automáticamente la clave SSH, debes agregarla al agente SSH:

1. Inicia el agente SSH (si no está en ejecución):

    ```bash
    eval "$(ssh-agent -s)"
    ```

2. Agrega tu clave privada al agente SSH:

    ```bash
    ssh-add ~/.ssh/id_rsa
    ```

¡Listo! Ya tienes tu clave SSH generada y lista para ser usada.