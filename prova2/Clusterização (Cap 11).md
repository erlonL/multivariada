# Clusterização
Análise de clusters pode ser divido em dois passos fundamentais:
1. Escolha do método de cálculo de distância
	- Checar cada par de observações e suas similaridades. Uma métrica de similaridade é definida para determinar o quão perto estão os pares. No fim quanto mais próximos estão os pares, mais homogêneos são.
	1. Euclideana
	2. Manhattan
	3. Quadrática (**checar!**)
3. Escolha do algoritmo de agrupamento
	- É escolhido um algoritmo tal que a distância entre grupos seja a máxima possível, e a entre observações seja a mínima possível.
	1. Abordagem top-down ou divisiva
	2. Abordagem bottom-up ou aglomerativa (**checar!**)

## Proximidade entre objetos
O começo de uma análise de clusters é uma matriz de distância $X(n, p)$ com $n$ observações para $p$ colunas (ou variáveis). A proximidade entre esses objetos (ou linhas), é definido pela matriz de distância $D(n \times n)$.
A matriz $D$ contém métricas de *similaridade* ou *dissimilaridade* entre $n$ objetos. Se os valores $d_{ij}$ são distâncias, então quanto maior o valor, menos parecidos são os objetos e então eles medem dissimilaridade. Se os valores $d_{ij}$ são métricas de proximidade, então quanto maior o valor, mais similares são os objetos.
Podemos ter o *caso binário* (que gera valores de proximidade) e o *caso contínuo* que geralmente dão em matrizes de distância.
### Caso Binário
No caso binário, vamos usar a mesma lógica de comparar pares de observações $(x_{i}, x_{j})$ onde $x_{i}^{T}= (x_{i1}, \dots, x_{ip})$,  $x_{j}^{T}= (x_{j1}, \dots, x_{jp})$ e $x_{ik}, x_{jk} \in \{0, 1\}$ (binários). Então, temos apenas 4 casos:
$$\begin{cases}
x_{ik} = x_{jk} = 1  \\
x_{ik} = 0, x_{jk} = 1   \\
x_{ik} = 1, x_{jk} = 0   \\
x_{ik} = x_{jk} = 0  
\end{cases}$$
Definindo, 
$$\begin{cases}
a_{1} = \sum_{k=1}^{p}I(x_{ik} = x_{jk} = 1) \\
a_{2} = \sum_{k=1}^{p}I(x_{ik} = 0, x_{jk} = 1) \\
a_{3} = \sum_{k=1}^{p}I(x_{ik} = 1, x_{jk} = 0)  \\
a_{4} = \sum_{k=1}^{p}I(x_{ik} = x_{jk} = 0)
\end{cases}$$
Onde $I$ define-se como a indicatriz de um conjunto de observações.  
$$I_{A}(x) = \begin{cases}
1, x \in A  \\
0, x \not\in A
\end{cases}$$
Basicamente, a indicatriz define se uma observação pertence ou não a um cluster específico.
A métricas de proximidade utilizadas são definidads com base na seguinte expressão:
$$d_{ij} = \frac{a_{1} + \delta a_{4}}{a_{1} + \delta a_{4} + \lambda(a_{2} + a_{3})}$$
onde $\delta$ e $\lambda$ são fatores de peso.
![[Pasted image 20240905165044.png]]
### Caso Contínuo
Para o caso contínuo podemos usar as métricas de norma $L_{r}$, $r\ge 1$, como a distância euclideana, norma $L_{1}$ ou euclideana ao quadrado.
Em casos que as variáveis não estão na mesma escala, uma *padronização* deve ser aplicada.
## Algoritmos de Clusterização
Existem dois tipos essenciais de métodos de clusterização: **algoritmos hierárquicos** e de **particionamento**.  Algoritmos hierárquicos podem ser divididos em *aglomerativos* e *divisivos*.
- aglomerativo: começa definindo cada observação como um cluster e vai agrupando os mais próximos.
- divisivo: começa com um cluster que contém todas as observações e vai dividindo de acordo com a dissimilaridade entre elas.
Os algoritmos de partição começam com uma dada definição de grupo e vão trocando os elementos até que uma certa pontuação seja alcançada.
### Algoritmos hierárquicos e divisivos
Algoritmo clusterização hierárquica
```
1. Construa a matriz de n elementos
2. Compute a matriz de distância D

FAÇA
3. Ache os dois clusters com a menor distância entre si
4. Agrupe esses dois clusters
5. Compute a matriz de distâncias entre os novos grupos e obtenha uma matriz de distância reduzida D
ENQUANTO todos os clusters estão aglomerados em X
```
Se dois novos grupos $P + Q$ estão unidos, podemos computar a distância entre esses grupos unidos e um grupo $R$ com base na seguinte função de distância:
$$d(R, P+Q) = \delta_{1}d(R, P) + \delta_{2}d(R, Q) + \delta_{3}d(P, Q) + \delta_{4}|d(R, P) - d(R, Q)|$$
Os $\delta_{j}$'s são fatores de peso que, dependendo de como são alterados, resultam em diferentes algoritmos de clusterização. Estes estão definidos a seguir:
![[Pasted image 20240905171402.png]]
onde $n_{P} = \sum_{i=1}^nI(x_{i} \in P)$ é o número de objetos no grupo $P$. Os valores para $n_{Q}$ e $n_{R}$ são definidos analogamente.

"""
When there are more data points than in the example above, a visualization of the implication of clusters is desirable. A graphical representation of the sequence of clustering is called a **dendrogram**. It displays the observations, the sequence of clusters and the distancs between the clusters. *The vertical axis displays the indices of the points, whereas the horizontal axis gives the distance between the clusters.* Large distances indicate the clustering of heterogeneous groups. Thus, if we choose to “cut the tree” at a desired level, the branches describe the corresponding clusters.
"""
Um método de visualizar a sequência de separação dos clusters é chamado de **dendograma**, que mostra as observações, a sequência dos clusters e as distâncias entre os clusters. *O eixo vertical mostra os índices dos pontos e o eixo horizontal mostra a distância entre os clusters.* Grandes distâncias indicam grupos heterogêneos. Se escolhemos "cortar a árvore" em um certo nível, temos que os galhos são os clusters correspondentes.

#### Definições de algoritmos
Para o algoritmo de *single linkage*, a distância entre dois grupos é definida como o menor valor entre as distâncias individuais entre cada elemento do grupo. No caso:
$$d(R, P + Q) = min\{d(R, P), d(R, Q)\}.$$
Esse algoritmo também é chamado de *Nearest Neighbor algorithm*.

O algoritmo de *complete linkage*, define a distância entre dois grupos como o maior valor, entre as distâncias individuais. No caso:
$$d(R, P + Q) = max\{d(R, P), d(R, Q)\}.$$
Também chamado de *Farthest Neighbor algorithm*.

O algoritmo de *average linkage*, utiliza uma distância média:
$$d(R, P + Q) = \frac{n_{P}}{n_{P} + n_{Q}}d(R, P) + \frac{n_{Q}}{n_{P} + n_{Q}}d(R, Q)$$

O *algoritmo de centróide* é um pouco similar ao de *average linkage* e usa a distância geométrica natural entre $R$ e o centro de gravidade ponderado de P e Q.
$$d(R, P + Q) = \frac{n_{P}}{n_{P} + n_{Q}}d(R, P) + \frac{n_{Q}}{n_{P} + n_{Q}}d(R, Q) - \frac{n_{P}n_{Q}}{(n_{P} + n_{Q})^2}d(P, Q)$$

O *algoritmo ward* é diferente dos algoritmos de linkage, pois ele tem o objetivo de agrupar observações não apenas na proximidade, mas tendo como premissa não aumentar a variação dentro dos grupos drasticamente (analisando a heterogeneidade), ou seja, os grupos resultantes são os mais homogêneos possíveis. A *heterogeneidade* de um grupo $R$ é definida pela *inércia* dentro desse grupo, com a fórmula:
$$I_{R} = \frac{1}{n_{R}}\sum_{i=1}^{n_{R}}d^2(x_{i}, \bar{x}_{R})$$
Onde $\bar{x}$ é o centro de gravidade (média / ponto central) sobre os grupos. $I_{R}$ define uma métrica escalar da dispersão no grupo a partir do centro de gravidade. Se a distância euclideana é utilizada, então $I_{R}$ representa a soma das variâncias dos $p$ componentes de $x_{i}$ dentro de um grupo $R$