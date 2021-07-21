Sistema de recomendación de contenidos


El desafío


Desarrollar un sistema de recomendación para predecir qué nuevos contenidos, no previamente vistos, son más probables a ser elegidos para ver en Flow por un grupo de usuarios en base a su historial de visualizaciones.


Exploración del Dataset


Para comenzar, se realizó una exploración sobre los dos archivos que fueron proporcionados por la empresa Telecom. 
En el primero, train.csv, se pudo observar la información con relación a los usuarios, es decir, su número de cuenta, los shows (películas, series, programas de T.V., etc.) y la fecha y hora en la que los vio. Este Dataset contaba con 3.657.801 registros.
Por otro lado, el archivo metadata.csv nos aportaba información sobre los shows, como por ejemplo el título, el género, las palabras clave, los actores, etc. En total contenía 33.143 registros.
        -	https://github.com/Datathon2021/Recomendador


Data 


Atributos que se consideraron relevantes para el modelo (justificación)

Se empezó haciendo una exploración de los datos para observar qué columnas eran necesarias para el análisis en cuestión. Después de tomar en cuenta todos los datos, se decidió que la columna "customer_id" no es relevante para el análisis. Se utilizó la información con relación a "account_id" del dataset train.csv. 
Por otro lado, si se observa el dataset metadata.csv, este si sufrió varias alteraciones en cuanto al archivo original. Se empezó por eliminar unos pocos registros que no tienen content_id porque no nos servían para el sistema de recomendación. Además, se borraron las columnas que tienen muchos nulos. Las columnas que se utilizaron para el modelo fueron las siguientes: [Variables que quedaron dentro del dataset]. Eliminamos los content_id duplicados porque hacen referencia a elementos que no aportan nada al análisis. Por ejemplo, los episodios de una serie tienen distinto asset_id, pero el mismo content_id. Entonces, con quedarnos con un solo episodio ya alcanza.
Por último, se arreglaron varias categorías que estaban mal escritas o que les faltaba algún detalle dentro del dataset.


Modelo


Matriz de similaridad usando distancia de coseno

Para generar las recomendaciones, consideramos que lo que debíamos hacer era determinar qué tan similares eran entre sí los distintos espectáculos. Para lograrlo necesitábamos alguna medida de similitud, así que nos decidimos a utilizar la distancia de coseno (que es 1 - similaridad de coseno). La similaridad de coseno compara dos vectores de k componentes y nos arroja el coseno del ángulo entre ellos en ese espacio k-dimensional.
La fórmula de similaridad de coseno es la siguiente:
K(X, Y) = <X, Y> / (||X||*||Y||)

Explicación del modelo. Para un usuario i se obtenían las M películas vistas y se las comparaba con cada una de las N películas, obteniendo un coeficiente $c_{jk}$.

De esta forma obtuvimos una matriz de NxN (siendo N la cantidad de asset_id únicos) con valores que iban del 0 al 1 y nos decían qué tan similar era un show a cada uno de los otros shows que teníamos.
Una vez creada la matriz de similaridad pudimos filtrarla para quedarnos solo con las filas correspondientes a los M shows que había visto un usuario y sumar esas M filas para cada una de las N columnas. De esta forma, conseguimos N resultados que representaban qué tan similar era cada show disponible en la plataforma a todos los shows que el usuario había visto. Si, por ejemplo, una persona había visto 10 shows distintos y algún otro show S disponible en la plataforma era bastante similar a esos 10 shows ya vistos, el puntaje para S hubiera sido cercano a 10.

