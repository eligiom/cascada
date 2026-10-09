# cascada

Gestor de ficheros para la terminal: columnas en cascada como las del Finder del Mac, con la pinta
de los programas de MS-DOS (PC Tools, Norton Commander): fondo azul, marcos dobles y barra de teclas
de función abajo.

```
cascada            # abre la carpeta en la que estás
cascada ~/repos    # o la que le digas
```

```
 CASCADA  ~/fix/proyecto/src                                                 15,0 GB libres │ 23:06
╔═ proyecto ═══════════╤═ src ═════════════════════════════════╤═ main.py ═════════════════════════╗
║ src                ▸ │ util                                ▸ │ #!/usr/bin/env python3            ║
║ README.md            │ main.py                        1,2 KB │ """Hola."""                       ║
║                      │                                       │ import os                         ║
║                      │                                       │                                   ║
║                      │                                       │ # comentario con eñe              ║
║                      │                                       │ def suma(a, b=2):                 ║
║                      │                                       │     return a + b  # tab           ║
║                      │                                       │                                   ║
║                      │                                       │ class Cosa:                       ║
║                      │                                       │     x = "cadena"                  ║
║                      │                                       │     n = 42                        ║
╚══════════════════════╧═════════════════════════════════ 2/2 ═╧══════════════ Python · 88 líneas ═╝
 -rw-rw-r--  paquirrin  1,2 KB  09/10/2026 22:51  main.py
1Ayuda    2Renom    3Ver      4Editar   5Copiar   6Mover    7MkDir    8Borrar   9Sitios   10Salir
```

Cada columna es una carpeta del camino; la última enseña lo que hay bajo el cursor: el contenido si
es una carpeta y, si es un fichero, una vista previa:

| Fichero | Qué se ve |
| --- | --- |
| Texto y código | El contenido, con colores de sintaxis |
| Imágenes | La imagen, dibujada con medios bloques y 16 colores (como en una EGA) |
| PDF | El texto de las dos primeras páginas |
| Comprimidos (zip, tar, 7z, deb…) | La lista de lo que llevan dentro |
| Lo demás | Qué es (`file`) y un volcado hexadecimal |

Cuantas más columnas de ancho tenga la ventana, más carpetas del camino caben (hasta tres, además
de la actual y la vista previa). El mínimo es 48×10.

## Teclas

`F1` (o `?`) las enseña dentro del programa. Si la terminal se queda con alguna tecla de función
(GNOME suele quedarse `F1` y `F10`), cada una tiene una letra que hace lo mismo.

| Tecla | Qué hace |
| --- | --- |
| `↑` `↓` `←` `→` (o `k` `j` `h` `l`) | Moverse: `→` entra en la carpeta, `←` vuelve a la de arriba |
| `Intro` | Entra en la carpeta o abre el fichero con su programa |
| `/` | Filtra la lista según escribes |
| `Esc` | Quita el filtro; si no lo hay, las marcas; si no, lo copiado |
| `.` | Enseña u oculta los ficheros ocultos |
| `s` | Cambia el orden: nombre, fecha, tamaño, tipo |
| `~` · `-` · `g` | Carpeta personal · carpeta anterior · ir a una ruta (`Tab` completa) |
| `F9` (`u`) | Sitios: favoritos y discos montados |
| `b` | Pone o quita la carpeta actual de favoritos |
| `Espacio` · `a` · `*` | Marca y baja · marca todo · invierte las marcas |
| `F2` (`r`) | Renombrar |
| `F3` (`v`) | Ver a pantalla completa (`/` busca, `n` siguiente) |
| `F4` (`e`) | Editar con `$EDITOR` (si no hay, `nano`) |
| `F5` · `F6` | Copiar · mover, escribiendo el destino |
| `c` · `x` · `p` | Copiar · cortar · pegar en la carpeta en la que estés |
| `F7` (`n`) | Carpeta nueva |
| `F8` (`Supr`, `d`) | A la papelera (pregunta antes) |
| `Mayús+Supr` | Borrar definitivamente (pregunta, y por defecto es que no) |
| `S` (`!`) | Shell en esta carpeta; con `exit` se vuelve |
| `T` | Tema: `dos`, `noche`, `ambar`, `fosforo` |
| `R` (`Ctrl+L`) | Vuelve a leer las carpetas y repinta |
| `F10` (`q`) · `Q` | Salir · salir sin cambiar de carpeta |

El ratón también vale: un clic selecciona (en cualquier columna), doble clic abre, la rueda
desplaza y la barra de abajo se puede pulsar. Para seleccionar texto de la pantalla con el ratón
hay que mantener `Mayús`, como en cualquier programa de terminal que use el ratón.

Las operaciones actúan sobre lo marcado o, si no hay marcas, sobre el fichero del cursor.

## Que la terminal se quede donde estabas

Un programa no puede cambiar la carpeta de la terminal que lo lanzó. Para que al salir con `q` te
quedes en la carpeta por la que ibas, añade esta línea a `~/.bashrc`:

```bash
eval "$(command cascada --shell)"
```

Con `Q` (mayúscula) se sale sin cambiar de carpeta.

## Cuidados que tiene

- `F8` manda a la papelera (`gio trash`), nunca borra sin más. Solo si la papelera no puede con ello
  (un disco sin papelera, por ejemplo) ofrece borrar definitivamente, y hay que decirle que sí.
- Al copiar o mover, si algo ya existe en el destino pregunta: conservar ambos (`nombre (copia).ext`),
  sobrescribir (solo fichero sobre fichero) o saltar. Nunca mezcla carpetas por su cuenta.
- Al mover entre discos, el original solo se quita cuando su copia ha salido entera.
- No calcula tamaños de carpetas ni entra en ellas por su cuenta, así que no se cuelga recorriendo
  un disco de red.

## Instalación

Es un único fichero de Python 3 sin dependencias obligatorias:

```bash
git clone https://github.com/eligiom/cascada.git
ln -s "$PWD/cascada/cascada" ~/.local/bin/cascada
```

Si están, aprovecha: `pygments` (colores de sintaxis), `Pillow` (imágenes), `pdftotext`, `7z`,
`file`, `gio` y `xdg-open`. La vista de imágenes necesita una terminal de 256 colores; en las de
8 colores o monocromas todo lo demás funciona igual.

Recuerda el tema, el orden, los ocultos y los favoritos en `~/.local/state/cascada/estado.json`.

## Licencia

[MIT](LICENSE).
