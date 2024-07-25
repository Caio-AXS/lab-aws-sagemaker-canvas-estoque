# 📊 Previsão de Estoque Inteligente na AWS com [SageMaker Canvas](https://aws.amazon.com/pt/sagemaker/canvas/)

Bem-vindo ao desafio de projeto "Previsão de Estoque Inteligente na AWS com SageMaker Canvas". Meu nome é Caio Souza e a ideia desse projeto foi testar a capacidade dessa ferramenta No-code para resolução de problemas de ML.

## 🎯 Objetivos Deste Desafio de Projeto (Lab)

![image](https://github.com/digitalinnovationone/lab-aws-sagemaker-canvas-estoque/assets/730492/72f5c21f-5562-491e-aa42-2885a3184650)

## 🚀 Passo a Passo

### 1. Selecionar Dataset

-   Dataset utilizado: ![dataset-1000-com-preco-promocional-e-renovacao-estoque.csv](https://github.com/Caio-AXS/lab-aws-sagemaker-canvas-estoque/blob/main/datasets/dataset-1000-com-preco-promocional-e-renovacao-estoque.csv)

### 2. Construir/Treinar

-   Não houve a nessecidade de dropar/tratar dados, nulos ou duplicados pois o dataset já estava tratado inicialmente
-   O treinamento se deu com: 'QUANTIDADE_ESTOQUE' como target, 'ID_PRODUTO' como Item ID, 'DATA-EVENTO' como Timestamp
-   Foram usadas 900 linhas aleatórias para o mesmo
-   A previsão requisitada foi de apenas 1 dia
-   A construção se deu via 'QUICK BUILD'

### 3. Analisar

### 3.1. Indicadores utilizados
-   **Average Weighted Quantile Loss (wQL)** - Avalia a precisão das previsões em diferentes quantis da distribuição (P10, P50, P90).
Interpretação: Um valor menor indica um modelo mais preciso. No nosso caso, obtivemos um wQL de 0.060, sugerindo uma boa precisão do modelo em diferentes níveis da distribuição.
-   **Mean Absolute Percent Error (MAPE)** - Mede o erro percentual absoluto médio entre os valores previstos e os valores reais.
Interpretação: Um valor menor indica um modelo mais preciso. Obtivemos um MAPE de 0.148, o que indica que, em média, os valores previstos diferem dos valores reais em 14.8%. Este é um bom resultado, pois valores de MAPE abaixo de 20% são geralmente considerados aceitáveis em muitos contextos.
-   **Weighted Absolute Percent Error (WAPE)** - Mede a desvio total dos valores previstos em relação aos valores observados.
Interpretação: Um valor menor indica um modelo mais preciso. Obtivemos um WAPE de 0.100, mostrando que os erros absolutos representam 10% dos valores reais, indicando uma boa precisão geral do modelo.
-   **Root Mean Square Error (RMSE)** - É a raiz quadrada da média dos erros quadráticos.
Interpretação: Um RMSE menor indica um modelo mais preciso. Obtivemos um RMSE de 5.765, sugerindo que a dispersão dos erros é relativamente baixa.
-   **Mean Absolute Scaled Error (MASE)** - Normaliza o erro absoluto médio da previsão pelo erro absoluto médio de um método de previsão básico.
Interpretação: Um MASE < 1 indica que o modelo é melhor que o modelo básico. Obtivemos um MASE de 0.301, o que demonstra que o modelo é bem mais eficaz do que um método simples, como a média histórica.


### 3.2. Análise

-   Avg. wQL (0.060): Indica que o modelo possui uma boa precisão em diferentes pontos da distribuição (quantis P10, P50, P90). Isso sugere que o modelo consegue capturar bem a variação dos dados em diversos níveis.

-   MAPE (0.148): Em média, os valores previstos diferem dos valores reais em 14.8%. Este é um bom resultado, pois valores de MAPE abaixo de 20% são geralmente considerados aceitáveis em muitos contextos.

-   WAPE (0.100): Os erros absolutos representam 10% dos valores reais, o que indica uma boa precisão geral do modelo.

-   RMSE (5.765): A dispersão dos erros é relativamente baixa. Um RMSE menor implica que os erros são menos dispersos em torno da média, indicando uma previsão mais precisa.

-   MASE (0.301): Por ser significativamente menor que 1, o modelo supera a precisão de um método de previsão básico. Isso demonstra que o modelo é bem mais eficaz do que um método simples, como a média histórica.

-   **Conclusão**: Os resultados obtidos para os indicadores de previsão indicam que o modelo é altamente preciso e confiável. Os valores baixos para Avg. wQL, MAPE, WAPE, RMSE e MASE mostram que o modelo possui uma boa capacidade de previsão, com erros relativamente pequenos e uma alta precisão em comparação com métodos básicos de previsão. Esses indicadores juntos confirmam a eficácia do modelo para previsões robustas e precisas

-   A coluna com mais impacto dentre os resultados foi a coluna de 'PRECO' tendo 9,61% do impacto relativo

### 4. Prever

-   Com o modelo montado pode-se prever os resultados e daremos o exemplo de um dos índices estudados(1006 e 1022):
-   ![image](https://github.com/user-attachments/assets/269e822e-5d29-4168-bc63-9473e6212ca4)
-   ![single_prediction_results](https://github.com/user-attachments/assets/28b6671f-6e6f-44cd-aefa-b22f81458b11)
-   Os gráficos acima apresenta previsões para a data específica 2024-02-09 com três linhas diferentes representando os quantis P10, P50 e P90.
-   Linha Amarela (P90): Representa o valor acima do qual 10% das previsões se situam (valor: 70.497 e 99.999).
-   Linha Verde (P50): Representa a mediana das previsões, o ponto onde há 50% de chance de o valor real ser maior ou menor (valor: 62.968 e 94.725).
-   Linha Rosa (P10): Representa o valor abaixo do qual 10% das previsões se situam (valor: 55.967 e 1.672).
-   1006 - Tendência Descendente: Todas as três linhas(desta previsão) mostram uma tendência descendente à medida que se aproximam da data 2024-02-09, indicando uma expectativa de queda no valor do parâmetro previsto.
-   1022 - Espaçamento Entre as Linhas: A variabilidade ou incerteza nas previsões é ilustrada pelo espaçamento entre as linhas P10, P50 e P90. Uma maior distância sugere maior incerteza como entre a P10 e a P50, enquanto uma distância menor indica previsões mais consistentes, P50 e P90.

### 5. Conslusões

-   Tendo em vista as características do modelo, como variação de estoque entre 1 e 100, que são respectivamente os limites mínimo e máximo, é totalmente coerente as previsões se manterem dentro dessas margens
-   Apresentou 80% de certeza que a próxima medição pode estar entre 70 e 55 ou seja, 15 unidades de spread. O que pode fecilitar significativamente a área comercial para dimensionar um possível estoque para ser vendido para o próximo dia
-   Além disso o modelo foi capaz de perceber que devido ao fato do estoque estar próximo ao limite máximo sua tendência seria de cair e de que não houvessem entregas próximas, tanto que todos os quantis da amostra 1006 apontavem queda de quantidade, que na amostra do gráfica no dia 2024-02-08 apresentava 74 unidades e o o maior dos quantis representa menos de 71 unidades. Enquanto, na amostra 1022 devido ao estoque ser 9 gerou maior incerteza se poderia cair mais antes de ser repreenchido mas com maiores chances de ir para o limite máximo do que continuar caindo

## Conclusões sobre a Ferramenta

A ferramenta Sagemaker se mostrou de grande valia pois democratiza o acesso ao MachineLearning, devido ao fato de não haver a necessidade de codar, e a programação ser um dos principais entraves para alguns profissionais se aventurarem no mundo digital.
Todavia, A AWS tem um formato de funcionamento peculiar e com certa complexidade quanto a cobranças e formas de desenvolvimento, além disso a indisponibilidade de utilização de outras línguas pode afetar a experiência de muitas pessoas. As ferramentas de programação como python possuem muito mais flexibilidade para lidar com os modelos e desenvolver ferramentas mais assertivas e sua produção pode-se equivaler em tempo às atividades no sagemaker devido a demora para entrega de resultados da ferramenta da Amazon.
Tendo essas questões em vista, ferramentar pré-fabricadas para análise de dados, mesmo com maior facilidade para análise, ainda tem diversos pontos de melhoria, mas já pode ser considerada uma via a ser seguida por algumas emrpesas e profissionais.
