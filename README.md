# Sistema de recomendación de contenidos


# El desafío


Desarrollar un sistema de recomendación para predecir qué nuevos contenidos, no previamente vistos, son más probables a ser elegidos para ver en Flow por un grupo de usuarios en base a su historial de visualizaciones.


# Exploración del Dataset


Para comenzar, se realizó una exploración sobre los dos archivos que fueron proporcionados. 
* En el primero, train.csv, se pudo observar la información con relación a los usuarios, es decir, su número de cuenta, los shows (películas, series, programas de T.V., etc.) y la fecha y hora en la que los vio. Este Dataset contaba con 3.657.801 registros.
* Por otro lado, el archivo metadata.csv nos aportaba información sobre los shows, como por ejemplo el título, el género, las palabras clave, los actores, etc. En total contenía 33.143 registros.
        -	https://github.com/Datathon2021/Recomendador


# Data 


## Atributos que se consideraron relevantes para el modelo

* Se realizó una exploración de los datos para observar qué columnas eran necesarias para el análisis en cuestión. Después de tomar en cuenta todos los datos, se decidió que la columna "customer_id" no es relevante para el análisis. Se utilizó la información con relación a "account_id" del dataset train.csv. 
* Por otro lado, el dataset metadata sufrió varias alteraciones en relación al archivo original. Se empezó por eliminar unos pocos registros que no tenían content_id porque no eran de utilidad para el sistema de recomendación. Además, se borraron las columnas que tenían muchos nulos. 
Las columnas que se utilizaron para el modelo fueron las siguientes: "released_year", "show_type", "country_of_origin", "category", "keywords", "run_time_min", "audience", "made_for_tv", "pack_premium_1", "pack_premium_2", "pay_per_view". Se eliminaron los content_id duplicados porque hacen referencia a elementos que no aportan al análisis, por ejemplo, los episodios de una serie tienen distinto asset_id, pero el mismo content_id. Por último, se corrigieron varias categorías que estaban mal escritas o que les faltaba algún detalle dentro del dataset.


# Modelo


## Matriz de similaridad usando distancia de coseno

Para generar las recomendaciones, se consideró que lo que se debía hacer era determinar qué tan similares eran entre sí los distintos shows. Para lograrlo era necesario determinar alguna medida de similitud, por lo que se decidió utilizar la distancia de coseno (definida como 1 - similaridad de coseno). La similaridad de coseno compara dos vectores de k componentes y nos arroja el coseno del ángulo entre ellos en ese espacio k-dimensional.
La fórmula de similaridad de coseno es la siguiente:

<a href="https://www.codecogs.com/eqnedit.php?latex=\large&space;\text{similaridad}=\text{cos}(\theta)=\frac{\vec{u}\cdot\vec{v}}{\left&space;\|&space;\vec{u}&space;\right&space;\|\left&space;\|&space;\vec{v}&space;\right&space;\|}" target="_blank"><img src="https://latex.codecogs.com/png.latex?\large&space;\text{similaridad}=\text{cos}(\theta)=\frac{\vec{u}\cdot\vec{v}}{\left&space;\|&space;\vec{u}&space;\right&space;\|\left&space;\|&space;\vec{v}&space;\right&space;\|}" title="\large \text{similaridad}=\text{cos}(\theta)=\frac{\vec{u}\cdot\vec{v}}{\left \| \vec{u} \right \|\left \| \vec{v} \right \|}" /></a>

## Explicación del modelo. 

* Para un usuario i se obtenían los M shows vistos. A partir de ellos, se accedía a la matriz de similaridad y se extraía la comparación de cada uno de los M shows respecto a la totalidad. De esta forma se obtenía una matriz de orden (M,N), denominada vistas_usuario. 
* Se decidió incorporar un parámetro que regule la importancia de la antigüedad con que cada usuario vio un show en particular, asignando un mayor peso a las vistas más recientes. Para regular el comportamiento de estos pesos se introdujo un hiperparámetro denominado "c" en el modelo que tenía en cuenta la sensibilidad de los mismos.
* Se procedió a multiplicar cada fila de la matriz de vistas de usuario por su correspondiente peso en función del show.
* Luego se realizó una suma por columnas para obtener los puntajes correspondientes para cada uno de los N shows para ese usuario. 
* A continuación se descartaron aquellos shows que el usuario ya había visto, y se los ordenó según su puntaje decreciente.
* Finalmente, se realiza la recomendación tomando los 20 primeros shows con puntaje mayor.


