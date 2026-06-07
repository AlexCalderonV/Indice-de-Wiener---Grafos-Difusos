# Indice-de-Wiener---Grafos-Difusos
En este repositorio se encuentran algunos algoritmos para calcular el índice de Wiener para grafos difusos, tanto haciendo uso de la definición clásica de Rosenfeld como de una más reciente. 


# Algoritmos para el Cálculo del Índice de Wiener en Grafos Difusos

Este repositorio contiene la implementación en Python de tres enfoques metodológicos para calcular el Índice de Wiener ($WI$) en grafos difusos $G = (\sigma, \mu)$: el **Algoritmo de Filtrado Estructural**, el **Algoritmo Clásico de Rosenfeld** y el **Algoritmo para el índice de Wiener Normalizado** (este último es empleado para analizar redes de inmiración ilegal como en otras tituaciones reales).

## 1. Contexto Teórico

El **Índice de Wiener** es un descriptor topológico que cuantifica la "compacidad" de una red. En grafos difusos, este cálculo varía según la métrica de distancia elegida y el tratamiento de las conexiones débiles ($\delta$-aristas).

### El Enfoque Moderno (Filtrado $\delta$)
Este algoritmo utiliza un subgrafo fuerte $G'$ derivado de la eliminación de aristas $\delta$-débiles. Es ideal para modelar redes donde la robustez de la conexión es prioritaria, eliminando "ruido" topológico.

### El Enfoque Clásico (Rosenfeld)
Se basa en la definición original de Rosenfeld, considerando todas las aristas presentes en el grafo ($G$). 
* **Aplicación Real:** Es fundamental en el modelado del **flujo de personas y redes sociales**, donde incluso las rutas más débiles, clandestinas o poco transitadas representan flujos reales de información o individuos. A diferencia de los modelos de ingeniería de redes (donde una conexión débil es inútil), en ciencias sociales estas rutas son cruciales porque el flujo humano encuentra cualquier "grieta" disponible para circular.

## 2. Descripción de los Algoritmos

### A. Algoritmo de Filtrado Estructural (Dijkstra + Max-Min)
Este método realiza un pre-procesamiento topológico antes del cálculo:
1. **Detección $\delta$:** Clasifica aristas basadas en la fuerza del camino más fuerte alternativo.
2. **Geodésica Difusa:** Implementa una variante de Dijkstra que prioriza: 
   - Menor número de saltos.
   - Menor suma de pesos ($\mu$) en caso de empate.

### B. Algoritmo Clásico (Rosenfeld)
Este método calcula distancias directamente sobre el grafo original:
1. **Topología Íntegra:** No elimina ninguna arista.
2. **Dijkstra Clásico:** Calcula la distancia como la suma mínima de pesos de membresía entre cualquier par de nodos.

### C. Wiener Normalizado
1. Se define una $\mu$ distancia
2. Se calcula el índice de Wiener considerando dicha distancia.
3. Se normaliza el valor encontrado.
---

## 3. Especificaciones Técnicas

### Seudocódigo (Rosenfeld)
```latex
\begin{algorithm}[H]
\caption{Cálculo del Índice de Wiener (Rosenfeld)}
\begin{algorithmic}[1]
\Require Grafo difuso $G = (\sigma, \mu)$
\State \textbf{Inicializar} $d_R(u, v) = \infty$ para todo par $(u, v)$
\For{cada vértice $v \in V$}
    \State $dist\_dict \gets \text{Dijkstra\_Rosenfeld}(G, v)$
    \State Actualizar $d_R$ para todas las parejas de nodos
\EndFor
\State $WI \gets \sum_{i<j} \sigma(v_i)\sigma(v_j) d_R(v_i, v_j)$
\State \Return $WI$
\end{algorithmic}
\end{algorithm}
````

## 4. Uso del Software 

El código está diseñado para ejecutarse en Jupyter Notebook.

**Requisitos:** pip install numpy networkx matplotlib ipywidgets

###  Información acerca de la lectura de los datos del grafo.

Los algoritmos requieren para su ejecución la información de un grafo difuso almacenado en un archivo de texto plano (.txt), dicho archivo deberá seguir la estructura siguiente:

*  Línea 1: *n* (número de nodos del grafo). El *primer* nodo del grafo será etiquetado como $V_0$ mientras que el *último* será $V_{n-1}$.

*  Línea 2: Cada uno de los valores de pertenencia de los *n* vértices del grafo, es decir, los valores σ, que deberán estar separados por un espacio en blanco.

* Líneas 3 en adelante: A partir de esta línea se da la información para cada arista del grafo siguiendo la estructura: *Nodo_Origen Nodo_Destino Valor_μ*, es decir, se escribe primero un número entre el cero (primer nodo) y el $n-1$ (último nodo), se deja un espacio en blanco, luego se escribe otro número entre el mismo rango, se deja otro espacio en blanco y finalmente se escribe el valor de dicha arista. 

  * A manera de ejemplo, 1    5    0.4, indica que el vertice $V_1$ (que sería el segundo vértice) está enlazado con el vértice $V_5$ y el valor de la arista es $0.4$.

En el siguiente enlace se pueden ejecutar, en Google Colab, los algoritmos: 

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1ZfazrI_9jomW1QYivI9nizVoWvxD-tN4?usp=sharing)

## 5. Consideraciones sobre los Resultados

### Discrepancia  
Los valores de $WI$ obtenidos por Rosenfeld suelen ser menores o iguales a los obtenidos por el modelo de filtrado, ya que el modelo clásico siempre considera los "atajos" débiles que el modelo moderno purga.

### Seguridad
Este repositorio emplea diccionarios de adyacencia, garantizando eficiencia $O(E \log V)$ y permitiendo que el cálculo sea escalable para redes de mayor complejidad.

Desarrollado para el análisis topológico de redes de flujo humano.


