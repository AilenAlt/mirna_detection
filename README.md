# Detección de pre-miRNAs a partir de aprendizaje automatico #
**Objetivo**: comparar modelos capaces de identificar potenciales secuencias pre-miRNAs en el genoma de *Drosophila melanogaster* a partir de sus caracteristicas, y diferenciarla de secuencias que no son pre-miRNAs. 

**Motivación**: La identificación de pre-miRNAs es fundamental para entender la regulación génica en organismos eucariotas y ha sido objeto de estudio en la biología molecular durante los últimos años. Sin embargo, la detección de estas secuencias puede ser difícil dada su corta longitud y que no codifican proteínas. Debido a esto, no es sencillo identificar pre-miRNAs usando métodos basados en alineamientos como BLAST. En este trabajo me propongo utilizar la ciencia de datos y el aprendizaje supervisado para generar modelos capaces de predecir si una secuencia, dadas sus caracteristicas, genera un pre-miRNA.
Las caracteristicas requeridas para realizar la clasificación - como energía libre liberada, la cantidad de nucleótidos formando ciertos patrones, etc - pueden ser extraidas a partir de las secuencias de las horquillas. La disponibilidad de un genoma de *Drosophila melanogaster* completamente secuenciado facilita esta tarea.

## Instalación ##
Los siguientes comandos permiten instalar las bibliotecas necesarias:

``` python
pip3 install matplotlib
pip3 install numpy
pip3 install pandas
pip3 install seaborn
pip3 install sklearn
```

## Corrida ##
El proyecto fue realizado haciendo uso del sistema operativo Ubuntu 20.04.4 LTS

El script *get_data.sh* permite descargar el dataset utilizado en este análisis.
Comando para ejecutar el script en una terminal terminal:

``` bash
bash get_data.sh
```
Alternativamente, es posible obtener el dataset utilizado descomprimiendo el archivo *dme.csv.gz*.

``` bash
gunzip dme.csv.gz
```

El jupyter notebook *predict_mirna.ipynb* fue ejecutado utilizando Visual Studio Code. El código no genera
archivos de salida, los resultados pueden ser visualizados al ejecutar el notebook.

## Software utilizado ##

En este proyecto se utilizó Python como lenguaje de programación, dada a su popularidad en el ámbito científico y a que cuenta con numerosas *libraries* para procesamiento de datos y machine learning. Las librerías utilizadas en este proyecto son las siguientes:

- *Matplotlib.pyplot* y *seaborn*: son *libraries* de visualización de datos en 2D para Python, que permite generar gráficas y visualizaciones de datos de alta calidad.

- *NumPy*: *library* para procesamiento numérico y científico de datos. Permite trabajar con vectores y matríces, y proporciona numerosas funciones matemáticas y estadísticas.

- *Pandas*: es una *library* que proporciona estructuras de datos y herramientas para el análisis de datos. Ofrece una amplia gama de funciones para la manipulación y limpieza de datos.

- *Scikit-learn*: *library* para aprendizaje automático, que proporciona una amplia variedad de algoritmos de clasificación, regresión y clustering, entre otros. Además, ofrece herramientas para la evaluación de modelos.

Estas librerías fueron seleccionadas debido a su popularidad en la comunidad de machine learning y a su facilidad de uso. Además, todas ellas son de código abierto y están disponibles de manera gratuita, lo que permite a cualquier persona utilizarlas y contribuir a su desarrollo.

## Base de Datos utilizada ##
La base de datos utilizada se corresponde con el trabajo publicado en el artículo "*Genome-wide hairpins datasets of animals and plants for novel miRNA prediction*". El dataset correspondiente a *D. melanogaster* contiene caracteristicas de secuencias capaces de adquirir una estructura de *hairpin*. Su clase indica si la secuencia es un pre-miRNA conocido en base a miRBase v21 (clase 1), o si la secuencia no posee función conocida hasta el momento (clase 0). La base de datos miRBase es una base de datos curada de secuencias de miARNs, que almacena datos correspondientes a miARNs identificados experimentalmente y predicciones computacionales. miRBase se actualiza regularmente para incluir nuevos datos y para mejorar la calidad de las anotaciones disponibles.

Este dataset fue seleccionado debido a que contiene un conjunto de datos bien definido y etiquetado, que permite entrenar y evaluar modelos de aprendizaje automático. 

## Entrenamiento y evaluación de los modelos de clasificación ##

Para evaluar el rendimiento de los modelos a utilizar empleo el *F1 score*.
El *F1 score* es una métrica de evaluación del rendimiento de un modelo de clasificación que combina la precisión (*accuracy*) y la exhaustividad (*recall*) de un modelo en un único valor. Es decir, el F1 score permite evaluar de forma general la precisión y exhaustividad de un modelo.

En el caso de un algoritmo de clasificación como KNN o Random Forest, el F1 score se utiliza para evaluar la precisión del modelo en la tarea de clasificación. Un F1 score alto indica que el modelo tiene un buen rendimiento en la clasificación de las observaciones en las clases correctas, mientras que un F1 score bajo indica que el modelo tiene dificultades para clasificar correctamente las observaciones.

La métrica *accuracy* no funciona bien cuando las clases están desbalanceadas como es en este caso. La mayoría de los *hairpins* no poseen una función conocida, así que es muy fácil para un modelo acertar diciendo que no tendran funcion (clase 0). Para problemas con clases desbalanceadas es mucho mejor usar precision, recall y F1 (que combina precisión y recall). 

A partir de los graficos en los que se evaluan los modelos es posible concluir que:
- Se obtienen valores de f1 similares a los obtenidos durante desarrollo. 
- Los dos modelos poseen valores similares de precisión, recall y f1. 
- En general, la dispersión de estos estimadores es menor para el modelo RF. 
- Sin embargo, la mediana de los estimadores es mayor para el modelo KNN, por lo cual podría considerarse 
que que este modelo tiene una mejor *performance*.

## Limitaciones ##
Es importante destacar que el proyecto tiene algunas limitaciones que podrían afectar los resultados y las conclusiones. Una de ellas es la sobrerrepresentación de la clase 0 en el conjunto de datos utilizado. Esta clase representa secuencias que codifican RNAs capaces de formar estructura de horquilla pero que no tienen una función de miRNA conocida. El desbalance de clases puede afectar la capacidad de los modelos para detectar correctamente pre-miRNAs de la clase 1 (secuencias que codifican pre-miRNAs con función identificada según miRBase).

Otra limitación importante es que dentro de la clase 0 podría haber RNAs que sí funcionan como pre-miRNAs, pero cuya funciones todavía no han sido descubiertas o caracterizadas. Por lo tanto, el uso de esta categoría como negativos verdaderos al entrenar y evaluar los modelos podría afectar la capacidad de los modelos para detectar correctamente pre-miRNAs desconocidos.

A su vez, solo se ajustaron los hiperparámetros **número de árboles** en el modelo de Random Forest y **k y weights** en el modelo de KNN. Es posible que otros hiperparámetros también tengan un impacto en la precisión y rendimiento de los modelos, pero no se exploraron en este proyecto. 

Es importante tener en cuenta estas limitaciones al interpretar los resultados del proyecto y considerar en futuros estudios la necesidad de utilizar conjuntos de datos más equilibrados y la posibilidad de incluir nuevas categorías en la clasificación de pre-miRNAs.

## Conclusión ##
En este proyecto se utilizó aprendizaje automático para la detección de pre-miRNAs en el genoma de *Drosophila melanogaster*. Se compararon dos modelos, KNN y Random Forest, en términos de su desempeño en la clasificación de secuencias como pre-miRNAs con función o secuencias que no son pre-miRNAs.

Los resultados mostraron que los dos modelos se desempeñaron de forma similar, con valores similares de precisión, *recall* y F1. Sin embargo, la mediana de los estimadores fue levemente mayor para el modelo KNN, lo que sugiere que este modelo puede ser ligeramente superior en términos de su desempeño general. Además, se observó que el modelo de Random Forest presentó una menor dispersión en los valores estimados de precisión, *recall* y F1.

En general, los resultados obtenidos indican que el uso de técnicas de aprendizaje automático para la detección de pre-miRNAs puede ser una estrategia efectiva. 