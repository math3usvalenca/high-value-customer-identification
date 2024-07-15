# High-value-customer-identification
Objetivo: Selecionar os mais valiosos clientes para formar um programa de fidelidade em uma tarefa de clusterização. | Objective: Select the most valuable customers to form a loyalty program in a clustering task.

## 1. Businesss Problem

##### *Disclaimer: O contexto e as perguntas de negócio foram criados por mim para representar um cenário real.*

Este projeto utiliza-se de um conjunto de dados com transações reais de um varejo não registrado sediado no Reino Unido. A base de dados pode ser encontrada no seguinte link: 
<a href="https://archive.ics.uci.edu/dataset/352/online+retail">UCI MAchine Learning Repository</a>

Para o problemo de negócio, vamos nomear a empresa de Athena.

A empresa Athena é uma Outlet Multimarcas que comercializa produtos de segunda linha de várias marcas a um preço menor por meio de um e-commerce. O time de marketing da Athena vai lançar um programa de fidelidade para os melhores clientes da base, chamado "Guild". Por esse motivo, foi requisitado ao time de dados uma seleção de clientes elegíveis ao programa, usando técnicas avançadas de manipulação de dados. Como resultado deste projeto é esperado a entrega de uma lista de pessoas elegíveis para participar do programa, junto com um relatório respondendo às seguintes perguntas:

- **Quem são as pessoas elegíveis para participar do programa Guild ?**
- **Quantos clientes farão parte do grupo?**
- **Quais as principais características desses clientes ?**
- **Qual a porcentagem de contribuição do faturamento, vinda do Guild ?**
- **Quais as condições para uma pessoa ser elegível ao Guild ?**
- **Qual a garantia que o programa Guild é melhor que o restante da base ?**
- **Quais ações o time de marketing pode realizar para aumentar o faturamento?**

## 2. Solution Stragegy

**Step 01 - Data Description**: Observar medidas resumo dos dados, como média, mediana, quartis, skewness e kurtosis.

**Step 02 - Features investigation**: Investigar valores atípicos nos dados para que não atrapalhem no processo de clusterização.

**Step 03 - Data preparation**: Filtrar variáveis e eliminar ruídos.

**Step 04 - Feature Engineering**: Etapa crucial, derivar features que ajudem no processo de agrupamento dos clientes.

**Step 05 - Dimensionality Reduction**: Quando os recursos não estão em uma escala semelhante, os recursos com valores maiores podem influenciar desproporcionalmente o resultado do agrupamento, levando potencialmente a agrupamentos incorretos. Irei aplicar um Scaler nos dados e irei utilizar as seguintes técnicas de redução de dimensionalidade: PCA, UMAP e Tree-Based embedding.

**Step 06 - Clustering Models**: Irei utilizar os seguintes algoritmos de clusterização: Gaussian Mixture Model, Hierarchical Clustering e K-Means.

**Step 07 - Clustering Models Avaliation**: Para avaliar a performance dos modelos irei utilizar as métricas Silhouette Score, Calinski Harabasz Score e Davies Bouldin Score.

**Step 08 - Cluster Analysis and Profile**: Esta etapa é essencial para validar a eficácia do agrupamento e para garantir que os agrupamentos sejam coerentes e bem separados. Irei observar as distribuições em cada cluster e depois traçar um perfil para cada um deles.

**Step 09 - Exploratory Data Analysis**: Realizar uma análise exploratória para validadar hipóteses e responder as perguntas de negócio. 

**Step 10 - Create a Report**: Criar um relatório com as principais métricas e características do cluster Guild para que as pessoas do marketing possam usar os resultados para alavancar o negócio.

## 3. Top 3 Data Insights

**Hypothesis 01:** Os clientes do cluster Guild possuem um volume (produtos) de compras acima de 10% do total de compras.

- Verdadeiro: 42% dos produtos foram comprados pelo clientes Guild.


**Hypothesis 02:** A mediana do faturamento do cluster Guild é 10% maior que a mediana do faturamento geral.

- Verdadeiro:  A mediana do cluster Guild é 491.85% maior que a mediana de faturamento geral.

**Hypothesis 07:** A mediana dos preços dos produtos comprados pelos clientes do cluster Guild é 10% maior do que a mediana de todos os preços dos produtos.

- Falso: A mediana dos preços dos produtos comprados pelos clientes do cluster Guild é 15% menor que a mediana de todos os preços dos produtos totais, o que já se torna um insight para o marketing (fazer estes clientes comprar produtos mais caros).

## 4. Clustering Model Performance

Abaixo está a tabela com a pontuação de Silhueta para os algoritmos utilizados.

| **Model and Space**       | **2 clusters** | **3 clusters** | **4 clusters** | **5 clusters** | **6 clusters** | **7 clusters** | **8 clusters** |
|---------------------------|----------------|----------------|----------------|----------------|----------------|---------------:|----------------|
| GMM in PCA                | 0.10           | 0.05           | 0.09           | 0.05           | 0.021          |          0.024 | 0.009          |
| GMM in UMAP               | **0.73**       | 0.45           | 0.50           | 0.48           | 0.50           |           0.51 | 0.48           |
| GMM in Tree embedding     | 0.43           | 0.47           | 0.39           | 0.41           | **0.51**       |           0.45 | 0.47           |
| HC in PCA                 | 0.20           | 0.25           | 0.24           | 0.24           | 0.22           |           0.22 | 0.20           |
| HC in UMAP                | **0.73**       | 0.44           | 0.48           | 0.46           | 0.48           |           0.48 | 0.47           |
| HC in Tree embedding      | 0.38           | 0.47           | 0.46           | 0.45           | 0.49           |           0.52 | **0.55**       |
| K-Means in PCA            | 0.24           | 0.28           | 0.28           | 0.27           | 0.26           |           0.25 | 0.23           |
| K-Means in UMAP           | **0.73**       | 0.45           | 0.51           | 0.49           | 0.51           |           0.52 | 0.50           |
| K-Means in Tree embedding | 0.42           | 0.48           | 0.47           | 0.50           | 0.52           |           0.54 | **0.56**       |

Pensando no problema de negócio, 2 clusters seria um número muito baixo. Então o modelo final escolhido foi o K-Means no espaço Tree embedding com um número de clusters igual a 8, cuja silhueta apresentou-se em 56%. Abaixo as métricas gerais do K-Means:
 
| **Métrica**               | **Valor**      | 
|---------------------------|----------------|
| Number of Observations    | 4121           |
| Silhouette Score          | 0.56           | 
| Calinski Harabasz Score   | 8629           |
| Davies Bouldin score      | 0.59           | 

- Um Silhouette Score em torno de 0.56, embora não seja tão próximo de 1, sugere uma separação satisfatória entre os clusters. Isso indica que os grupos são relativamente distintos, embora possam haver algumas sobreposições menores entre eles. Idealmente, uma pontuação mais próxima de 1 seria preferível, pois indicaria clusters mais claramente definidos e separados.

- A pontuação Calinski-Harabasz de 8629,78 é consideravelmente alta, o que indica uma boa definição dos clusters. Uma pontuação mais alta nessa métrica geralmente sugere uma definição mais clara dos clusters, o que implica que nosso algoritmo de clusterização conseguiu identificar uma estrutura significativa no espaço de dados utilizado.

- A pontuação Davies Bouldin de 0.59 é uma pontuação razovelmente baixa, indicando que os clusters estão bem separados e relativamente compactos, além de não serem muito similares.

## 5. Answers to business questions

### Quem são as pessoas elegíveis para participar do programa de Guild ?
- Com a clusterização foi possível encontrar uma lista de clientes para participar do programa Guild (dados salvos em csv).
### Quantos clientes farão parte do grupo?
- Um total de 449 clintes.
### Quais as principais características desses clientes ?
- 📝 Perfil: São os clientes que mais gastam com uma média de 3971 de total gasto. A recência é baixa (média de 27 dias). Compram uma diversidade grande de produtos e têm alta frequência de compra. Possuem uma elevada variação de gastos mensais, indicando que seus padrões de gastos são mais instáveis.Apesar de gastarem muito, este grupo também é o que mais cancela, indicando possíveis compras impulsivas. Estes clientes compram mais no meio da semana e durante a tarde.

### Qual a porcentagem de contribuição do faturamento, vinda do Guild ?
- Porcentagem de faturamento do cluster Guild é de 38.35%
  
### Quais as condições para uma pessoa ser elegível ao Guild ?
De maneira geral, não podemos dizer tais condições, apenas o modelo de clusterização pode fazer isso. Mas se quisermos nos basear nas características do cluster Guild, podemos citar:

- Total gasto dentro de determinado valor (R$ 3818 - R$ 4124).
- Recência baixa (ex.: compra recente dentro dos últimos 27 dias).
- Alta frequência de compra e diversidade de produtos adquiridos.
- Elevada variação de gastos mensais.


### Qual a garantia que o programa Guild é melhor que o restante da base ?
- Encontramos um cluster com gastos e frequência de compra elevados e uma baixa recência, mas uma garantia de sua eficácia só será possível ao observar a curva de contribuição deste cluster no tempo, quando comparado aos outros.
  
### Quais ações o time de marketing pode realizar para aumentar o faturamento?

Exemplos de ações que o time de marketing poderá realizar:

- Campanhas personalizadas: Enviar ofertas e promoções exclusivas para membros do Guild com base em seus produtos preferidos

- Engajamento e retenção: Melhorar a experiência de compra online e o suporte ao cliente para reduzir cancelamentos e devoluções

- Comunicação e feedback: Coletar feedback constante dos membros do Guild para aprimorar o programa e identificar áreas de melhoria

## 6. Conclusions

Dado o contexto do problema de negócio, podemos dizer que foi-se alcançado o que se esperava. Foram definidos 8 clusters, um deles para participar do programa Guild e outros clusters importantes que apresentaram clientes parecidos com o Guild e clientes em churn. Com estes resultados em mãos, certamente o time de marketing poderá tomar as decisões certas e precisas para aumentar o faturamento da empresa.
