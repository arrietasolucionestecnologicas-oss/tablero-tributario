# Tablero tributario: GitHub (pantalla) + Apps Script (servidor)

```
Celular / computador  →  GitHub Pages (index.html, solo la pantalla)
                              ↓ pide los datos con fetch
                         Apps Script (Code.gs + Datos.gs, todos los procesos)
                              ↓
                         Hoja de Google (hojas "Tarjetas" y "Estados")
```

Node.js no hace falta: la pantalla es HTML, CSS y JavaScript sin compilar, y Apps Script ya corre JavaScript en los servidores de Google. Como la página que ve el usuario es la de GitHub, no sale el aviso "Esta aplicación fue creada por un usuario de Google".

## A. Apps Script (servidor)

1. En Google Sheets crea una hoja de cálculo nueva, por ejemplo "Tablero tributario - datos".
2. Menú **Extensiones > Apps Script**.
3. Borra lo que haya en `Código.gs` y pega el contenido de `Code.gs`.
4. Crea otro archivo con el botón **+ > Secuencia de comandos**, llámalo `Datos` y pega el contenido de `Datos.gs`.
5. En `Code.gs` cambia `CAMBIA-ESTA-CLAVE` por una clave tuya (la escribirás una vez en cada dispositivo).
6. Arriba elige la función `inicializar` y pulsa **Ejecutar**. Acepta los permisos. Se crean las hojas `Tarjetas` y `Estados`.
7. **Implementar > Nueva implementación > Tipo: Aplicación web**
   - Ejecutar como: **Yo**
   - Quién tiene acceso: **Cualquier persona**
8. Copia la URL que termina en `/exec`.

Si después cambias `Code.gs`, usa **Implementar > Administrar implementaciones > editar > Nueva versión** para que el cambio se publique con la misma URL.

## B. GitHub (pantalla)

1. Crea un repositorio, por ejemplo `tablero-tributario`, y sube `index.html` a la raíz.
2. Edita `index.html`, busca `CONFIG` al inicio del script y pega la URL `/exec` en `API_URL`.
3. **Settings > Pages > Deploy from a branch > main / (root)**.
4. Al minuto abre `https://TU-USUARIO.github.io/tablero-tributario/`.
5. La primera vez en cada dispositivo te pide la clave. En el celular usa **Añadir a pantalla de inicio** del navegador.

## C. Uso

- Las marcas y los cambios de estado se guardan en la hoja `Estados`, así que celular y computador ven lo mismo.
- Hay 8 tarjetas sin fecha (dos fundaciones sin NIT y el consumo de Yulieth). Cuando tengas el NIT, escribe la fecha en la columna `fecha` de la hoja `Tarjetas`, como `2027-01-15` o `15/01/2027`. Aparece al recargar el tablero.
- Si pierdes conexión, la pantalla muestra los últimos datos y guarda tus marcas para enviarlas al volver.

## Seguridad

- La clave vive solo en `Code.gs` y en cada dispositivo. No la escribas en GitHub.
- La URL `/exec` queda visible en el repositorio, pero sin la clave no devuelve datos.
- Las tarjetas no incluyen NIT completos, solo el último dígito.
