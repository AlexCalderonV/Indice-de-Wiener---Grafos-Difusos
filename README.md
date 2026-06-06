# Indice-de-Wiener---Grafos-Difusos
En este repositorio se encuentran algunos algoritmos para calcular el índice de Wiener para grafos difusos, tanto haciendo uso de la definición clásica de Rosenfeld como de una más reciente. 


# Algoritmos para el Cálculo del Índice de Wiener en Grafos Difusos

Este repositorio contiene la implementación en Python de dos enfoques metodológicos para calcular el Índice de Wiener ($WI$) en grafos difusos $G = (\sigma, \mu)$: el **Algoritmo de Filtrado Estructural** y el **Algoritmo Clásico de Rosenfeld**.

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

### Entrada de Datos
El archivo .txt debe seguir este formato:

   I. **Línea 1:** $n$ (número de nodos). 

   II. **Línea 2**: Valores $\sigma$ separados por espacio.
   
   III. **Líneas 3+**: Nodo_Origen Nodo_Destino Valor_μ

## 5. Consideraciones sobre los Resultados

### Discrepancia  
Los valores de $WI$ obtenidos por Rosenfeld suelen ser menores o iguales a los obtenidos por el modelo de filtrado, ya que el modelo clásico siempre considera los "atajos" débiles que el modelo moderno purga.

### Seguridad
Este repositorio emplea diccionarios de adyacencia, garantizando eficiencia $O(E \log V)$ y permitiendo que el cálculo sea escalable para redes de mayor complejidad.

Desarrollado para el análisis topológico de redes de flujo humano.
