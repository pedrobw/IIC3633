## Semana 2

### [Matrix factorization techniques for recommender systems (2009). Koren, Y., Bell, R., & Volinsky, C.](https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf)

En este documento se discuten incialmente algunas diferencias entre los métodos de filtro colaborativo, como aquellos que discutí la semana pasada, y aquellos basados en el contenido, que pueden evitar los problemas de _cold start_ que presentan los primeros. Igualmente, se habla de algunas diferencias existentes entre el _feedback_ implícito y el _feedback_ explícito para recolectar información pertinente respecto a los gustos y a las preferencias de los usuarios con quienes se trabaja. Todo esto sirve como introducción para el tema que realmente se busca presentar: el uso de modelos matriciales que representen de manera adecuada las preferencias de los usuarios.

De aquí en adelante, al igual que en el documento, consideraremos una cantidad f de factores tomados en cuenta para un modelo, y, para un usuario u y un ítem i, denotaremos por r<sub>ui</sub> el rating dado por el usuario u al ítem i, por q<sub>i</sub> en R<sup>f</sup> el vector que representa la presencia de cada factor en el ítem i, y por p<sub>u</sub> en R<sup>f</sup> el vector que representa la afinidad del usuario u con cada uno de los factores. Los factores en cuestión pueden ser más o menos explícitos y dependerán de cada modelo. Se puede tener interpretaciones muy precisas de ellos, pero también interpretaciones completamente incomprensibles, escondidas bajo la descripción matemática del modelo.

El primer y más simple modelo, que sirve como base para los demás, es un modelo que considera una minimización del error cuadrático considerando una penalización sobre la norma de los vectores (con el fin de minimizar la cantidad de factores que explican el modelo).
> min ∑<sub>u,i </sub>(r<sub>ui</sub> - p<sub>u</sub><sup>T</sup>q<sub>i</sub>)<sup>2</sup> - λ(||p<sub>u</sub>||<sup>2</sup> + ||q<sub>i</sub>||<sup>2</sup>)

Esta simplificación es muy similar a la que se usa en una regresión tipo LASSO que estudié hace unos años en el curso de Métodos de Optimización, sin emabargo me parece algo lamentable que el _paper_ no se dé el tiempo de explicar la razón por la que se utiliza esta forma de minimización, y de esta forma se ve poco intuitiva.

Se proponen dos técnicas para resolver esta forma de problemas, la del descenso del gradiente y la de _alternating least squares (ALS)_ o mínimos cuadrados alternantes, ambas técnicas iterativas que convergen bastante rápido, siendo la primera la más simple y fácil de implementar y usualmente la rápida en matrices dispersas (_sparse_), y la segunda algo más compleja pero paralelizable y preferible cuando las matrices son más densas.

Los modelos siguientes son ya más interesantes y explican los potenciales de agregar variables que agregan información al modelo, como el bias b<sub>u</sub> de un usuario u (visto como la desviación en las calificaciones hechas por el usuario respecto al promedio de la base de datos) y el bias b<sub>i</sub> de un ítem (visto como la desviación en las calificaciones que recibe el ítem que se desea evaluar), o variables que incluyen información temporal o respecto de la confianza de la predicción, como muestran los modelos dados respectivamente por
> min ∑<sub>u,i</sub> (r<sub>ui</sub> - µ - b<sub>u</sub> - b<sub>i</sub> - p<sub>u</sub><sup>T</sup>q<sub>i</sub>)<sup>2</sup> - λ(||p<sub>u</sub>||<sup>2</sup> + ||q<sub>i</sub>||<sup>2</sup> + b<sub>i</sub><sup>2</sup> + b<sub>u</sub><sup>2</sup>)

> min ∑<sub>u,i</sub> (r<sub>ui</sub>(t) - µ - b<sub>u</sub>(t) - b<sub>i</sub>(t) - p<sub>u</sub><sup>T</sup>(t)q<sub>i</sub>(t))<sup>2</sup> - λ(||p<sub>u</sub>(t)||<sup>2</sup> + ||q<sub>i</sub>(t)||<sup>2</sup> + b<sub>i</sub><sup>2</sup>(t) + b<sub>u</sub><sup>2</sup>(t))

> min ∑<sub>u,i</sub> c<sub>ui</sub>(r<sub>ui</sub> - µ - b<sub>u</sub> - b<sub>i</sub> - p<sub>u</sub><sup>T</sup>q<sub>i</sub>)<sup>2</sup> - λ(||p<sub>u</sub>||<sup>2</sup> + ||q<sub>i</sub>||<sup>2</sup> + b<sub>i</sub><sup>2</sup> + b<sub>u</sub><sup>2</sup>)

Se concluye comentando que la utilización de estos métodos (y otros más sofisticados) han terminado por dominar el área de los sistemas recomendadores tras los buenos resultados mostrados en el Netflix Prize. Me parece que es algo razonable, puesto que a diferencia de otras descomposiciones matriciales más directas, estas se basan en un modelo de optimización, que de representar bien la realidad subyacente del comportamiento de los usuarios, efectivamente debiese ser logrado.

