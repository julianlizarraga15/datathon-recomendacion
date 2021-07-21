# Sistema de recomendación de contenidos

## Exploración del dataset

Para comenzar, se realizó una exploración sobre los dos archivos que fueron proporcionados por la empresa Telecom. 

En el primero, train.csv, se pudo observar la información con relación a los usuarios, es decir, su número de cuenta, los shows (películas, series, programas de T.V., etc.) y la fecha y hora en la que los vio. Este dataset contaba con 3.657.801 registros.

Por otro lado, el archivo metadata.csv nos aportaba información sobre los shows, como por ejemplo el título, el género, las palabras clave, los actores, etc. En total eran 33.143 registros.

## Data Wrangling
* Atributos eliminados
* Atributos que se consideraron relevantes para el modelo (justificación)
* Paso a variables dummies, booleanas, etc
* Nombre del dataset de usuarios y columnas que tiene 

## Modelo
* Matriz de similaridad usando distancia de coseno
* Explicación del modelo. Para un usuario i se obtenían las M películas vistas y se las comparaba con cada una de las N películas, obteniendo un coeficiente $c_{jk}$.

Para generar las recomendaciones, consideramos que lo que debíamos hacer era determinar qué tan similares eran entre sí los distintos shows. Para lograrlo necesitábamos alguna medida de similitud, así que nos decidimos a utilizar la distancia de coseno (que es 1 - similaridad de coseno). La similaridad de coseno compara dos vectores de k componentes y nos arroja el coseno del ángulo entre ellos en ese espacio k-dimensional.

La fórmula de similaridad de coseno es la siguiente:

K(X, Y) = <X, Y> / (||X||*||Y||)

De esta forma obtuvimos una matriz de NxN (siendo N la cantidad de asset_id únicos) con valores que iban del 0 al 1 y nos decían qué tan similar era un show a cada uno de los otros shows que teníamos.

Una vez creada la matriz de similaridad pudimos filtrarla para quedarnos solo con las filas correspondientes a los M shows que había visto un usuario y sumar esas M filas para cada una de las N columnas. De esta forma, conseguimos N resultados que representaban qué tan similar era cada show disponible en la plataforma a todos los shows que el usuario había visto. Si, por ejemplo, una persona había visto 10 shows distintos y algún otro show S disponible en la plataforma era bastante similar a esos 10 shows ya vistos, el puntaje para S hubiera sido cercano a 10. 
