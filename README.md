# datathon-recomendacion

# Exploración del dataset
Dos archivos:
* Train: con x usuarios, aclarando los atributos
* Metadata: información sobre los activos (peliculas), cantidad, por genero, etc

# Data Wrangling
* Atributos eliminados
* Atributos que se consideraron relevantes para el modelo (justificación)
* Paso a variables dummies, booleanas, etc
* Nombre del dataset de usuarios y columnas que tiene

# Modelo
* Matriz de similaridad usando distancia de coseno
* Explicación del modelo. Para un usuario i se obtenían las j películas vistas y se las comparaba con cada una de las k películas, obteniendo un coeficiente $c_{jk}$.
