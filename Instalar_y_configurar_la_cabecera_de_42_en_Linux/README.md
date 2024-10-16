# `Instalar y configurar la cabecera de 42 en Linux`
`
`
## Paso 1: Clonar el repositorio de 42header

Abre una terminal y clona el repositorio que contiene el script de la cabecera de 42:

```bash
git clone https://github.com/42Paris/42header.git
```

## Paso 2: Instalar el script

Ve al directorio del repositorio clonado:

```bash
cd 42header
```

Dentro de este directorio, encontrarás un archivo llamado `set_header.sh`, que es el script que instalará la cabecera de 42 en tu entorno. Ejecuta este script:

```bash
./set_header.sh
```

Este script agregará los archivos necesarios para que puedas usar el comando `stdheader` en **VIM**.

## Paso 3: Configurar tu nombre de usuario y correo electrónico

Después de instalar el script, necesitarás configurar tu nombre de usuario y correo electrónico para que aparezcan correctamente en la cabecera.
Para ello, debes editar tu archivo de configuración de **VIM** (`~/.vimrc`). Si no tienes este archivo, puedes crearlo. Abre **VIM** y edita este archivo con el siguiente comando:

```bash
vim ~/.vimrc
```

Dentro de este archivo, deberás añadir dos líneas donde puedes especificar tu nombre de usuario y tu correo electrónico:

```bash
let g:user42='tu_nombre'
let g:mail42='tu_email@student.42.f'
```

Guarda los cambios y cierra el archivo.

## Paso 3: Configurar la cabecera en VIM

##### 1.-  Editar el archivo `.vimrc`
`
`
Para que **VIM** inserte la cabecera automáticamente o mediante una combinación de teclas, debes editar nuevamente tu archivo de configuración de **VIM** (`~/.vimrc`): 

```bash
vim ~/.vimrc
```

##### 2.- Agregar el comando para insertar la cabecera
`
`
En el archivo `.vimrc`, agrega las siguientes líneas:

- Si quieres que la cabecera se inserte automáticamente en cada archivo nuevo:

```vim
autocmd BufNewFile * :Stdheader
```

- Si prefieres insertar la cabecera manualmente, mapea una tecla (como F1) para hacerlo:

```vim
nnoremap <F1> :Stdheader<CR>
```

Esto le dice a **VIM** que inserte la cabecera cuando crees un nuevo archivo o cuando presiones `F1`.

#### Paso 5: Verificar la instalación

Una vez configurado, puedes verificar que la cabecera se inserta correctamente. Abre **VIM** y crea un archivo nuevo o presiona `F1` si configuraste la tecla, y deberías ver una cabecera como esta:

```c
/* ************************************************************************** */
/*                                                                            */
/*                                                        :::      ::::::::   */
/*   file_name.c                                        :+:      :+:    :+:   */
/*                                                    +:+ +:+         +:+     */
/*   By: tu_nombre <tu_email@student.42.fr>          +#+  +:+       +#+        */
/*                                                +#+#+#+#+#+   +#+           */
/*   Created: 2024/10/15 10:00:00 by tu_nombre          #+#    #+#             */
/*   Updated: 2024/10/15 10:00:00 by tu_nombre         ###   ########.fr       */
/*                                                                            */
/* ************************************************************************** */
```
