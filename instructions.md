---
title: Instrucciones para construir y editar Environmental Computing.
---

El sitio web está desarrollado utilizando el generador de sitios estáticos [Hugo](https://hugodocs.info) y se implementa en [Netlify](netlify.com). Estamos utilizando el tema `learn` [disponible aquí en GitHub](https://github.com/matcornic/hugo-theme-learn).


# Construcción del sitio

A continuación, presentamos nuestras notas sobre el mantenimiento del sitio.

Implementamos el sitio mediante R, utilizando el paquete blogdown. Puedes ejecutar Hugo de forma independiente, pero suponemos que utilizas R.

```
install.packages("blogdown")
```

El sitio requiere una versión particular de Hugo

```
blogdown::install_hugo("0.91.2")
blogdown::check_hugo()
```

Para construir el sitio:

```
blogdown::serve_site()
```

- Esto abre una página web donde el sitio se reconstruye continuamente. Si creas un nuevo archivo `Rmd`, se renderiza automáticamente y se agrega al sitio.
- Las páginas construidas se despliegan en la carpeta `public`.
- Estar atento a los mensajes de error.

Para detener el servidor:

```
blogdown::stop_server()
```

## Reconstrucción completa

Blogdown solo renderiza las páginas cuando el archivo `rmd` falta o ha sido actualizado. Para activar una reconstrucción completa, puedes eliminar todos los archivos `html` y `.lock` de la carpeta `content` y luego ejecutar `blogdown::serve_site()`.


```
list.files("content", pattern=".lock", recursive=TRUE, full.names=TRUE) %>% file.remove()
list.files("content", pattern=".html", recursive=TRUE, full.names=TRUE) %>% file.remove()
```

Ten en cuenta que la renderización se detendrá si se encuentra un error. Por lo tanto, es importante que tengas instalados todos los paquetes relevantes. Esto se puede lograr con la siguiente llamada, que asegura que se instalen todas las dependencias enumeradas en DESCRIPTION:

```
devtools::install_deps()
```

# Escribiendo contenido

## Estructura de carpetas

El menú lateral se crea a partir de la estructura de archivos en la carpeta `content`. Tanto las páginas `Rmd` como `md` se renderizan en `html`.

Estamos utilizando una estructura donde cada nueva página se almacena en su propia carpeta, junto con todas las imágenes y datos que necesites. Dentro de esa carpeta, el archivo rmd se llama `_index.rmd`. Cualquier imagen o archivo vinculado en la página también se puede almacenar en la carpeta, lo que te permite usar enlaces relativos en el archivo rmd. Ejemplo:




```
├── page_name
|     ├── _index.rmd
|     ├── data.csv
|     ├── image.jpg
|     ├── ....
├── another_page
```


Creación de una nueva página:

1. Crea una nueva carpeta dentro de `content` (sin espacios ni caracteres especiales en el nombre).
2. Dentro de la carpeta, crea un nuevo archivo `_index.rmd` con yaml



```
---
title: "Making New Variables"
author: "Alistair Poore"
---
```

## Códigos cortos

Hugo utiliza códigos cortos para incrustar contenido fácilmente. Consulta https://gohugo.io/content-management/shortcodes/ (en inglés). Al renderizar archivos en blogdown, no puedes escribir los códigos cortos directamente, en su lugar, puedes utilizar la función [`shortcode`](https://rdrr.io/github/rstudio/blogdown/man/shortcode.html).

### Tweet
En un archivo `md`:

```
{{< tweet 306854385076543488 >}}
```

En un archivo Rmd:

```
`r blogdown::shortcode("tweet", "306854385076543488")`
```

### Figura
En un archivo `md`:


```
{{< figure src="https://danielfalster.com/images/2016.06.27-useR/useR-600x300.png" height="100" width="100" >}}
```

¿En un archivo `Rmd`?

```
`r blogdown::shortcode("figure", src = "https://danielfalster.com/images/2016.06.27-useR/useR-600x300.png", alt = "A nice figure")`
```



