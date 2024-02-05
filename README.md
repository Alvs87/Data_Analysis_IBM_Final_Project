Este laboratório faz parte do curso Data Science Professional Certificate da IBM.

Sumário
- Introdução
- Bibliotecas
- Primeira etapa: Entendimento do negócio
- Segunda etapa: Entendimento dos dados
- Terceira etapa: Preparaçao dos dados
- Quarta etapa: Modelagem


Introdução
Esta atividade se propõe a determinar o preço de mercado de uma casa, considerando um conjunto de recursos, inserida no contexto imobiliário de King County, localizado em Washington, EUA. 
Apesar de a atividade ter um escopo restrito, iremos fazer uma análise um pouco mais detalhada, para fins de entretenimento pessoal.
Analisaremos e preveremos preços de moradias usando atributos ou recursos como metragem quadrada, número de quartos, número de andares e assim por diante.
Por entretenimento, iremos utilizar a metodologia CRISP-DM nesta atividade, ainda que não exigida enquanto tarefa do curso da IBM, entretanto, iremos deixar de fora as duas ultimas etapas, de
avaliação, e, de aplicação por ser meramente uma simulação, e não possuímos dados e contexto suficiente para avaliar e aplicar.

Este caderno faz uso das bibliotecas
  - Pandas
  - Matplotlib
  - Seaborn
  - sklearn

Primeira Etapa: Entendimento do negócio
O mercado imobiliário é complexo, com diversos stakeholders, e cada um deles, com interesses distintos, e, por vezes, divergentes. No nosso contexto, iremos tomar a posição de um investidor.
Das métricas importantes para este contexto, podemos citar 
- Return on Investment(ROI)
- Net Operating Income (NOI)
- Internal Rate of Return (IRR)
- Cash Flow
- Gross Rent Multiplier (GRM)

Como atividade do curso, estas métricas não serão analisadas, ainda que importantes em outros contextos.
O escopo desta atividade é entender quais são as variáveis que mais impactam o valor de uma residência, e estimar a magnitude de tal impacto, para então entender como precificar o produto.

Segunda Etapa: Entendimento dos dados (se segura na cadeira, essa análise é longa)
No contexto do exercício, o dado nos foi entregue como um csv já anexado ao Notebook.

pd.read_csv
O primeiro comando dado foi a da leitura do arquivo CSV, para posterior análise.

df.head(), 
Em seguida este comando nos mostrou que nossa tabela possui dados que poderão vir a influenciar negativamente uma análise estatística, como as colunas 'sem nome' e 'id', pois não possuem um 
valor intrinsico, apenas de indexação, ou seja, organizacional.
Além disso, depreendemos que o arquivo nos serve ao propósito do escopo, tendo a coluna de preço e variáveis que fazem sentido, como numero de banheiros, quartos, metragem, andares, etc.

df.types
Este comando nos retornou que todos os dados obtidos são numéricos, indicando que não há necessidade do uso de 'dummy variables', que atribuiriam valores (booleanas, do tipo verdadeiro e falso) a colunas
contendo strings (texto).

df.describe()
O comando nos dá uma descrição estatística dos dados. 
Olhando para a tabela de descrição estatística, olhando para a variável 'waterfront', notamos que estes dados deveriam ser booleanos (sim e não), mas já foram tratados, pois sua coluna apresenta dados 1 e 0.

Terceira Etapa: Preparaçao dos dados
df.drop()
Apesar de os dados já estarem bem organizados, ainda precisamos consertar alguns vícios. Por exemplo, as colunas 'sem nome' e 'id' precisam ser eliminadas, uma vez que elas são apenas índices e seu valor
não corresponde a alguma grandeza. A função df.drop() realizará esta ação para nós.

isnull()sum()
Em teoria, deveríamos rodar esta função para todas as variáveis para identificar se possuímos dados faltantes, mas no contexto da atividade, apenas verificamos para banheiro e quarto, obtendo respectivamente
10 e 13 valores em branco. Para tratar estes dados iremos substituir os valores em branco pela média de suas respectivas colunas.

sns.boxplot()
Esta análise produz um gráfico 'box and whiskers', em que é possível identificar quais variáveis possuem mais 'outliers'. No caso presente, fizemos a análise segundo a localização a beira-mar ou não. Segundo o 
dado, é possível notar que os prédios residenciais que não estão a beira-mar possuem 'outliers' em proporção muito maior do que as de beira-mar.

sns.regplot()
Esta função cria um gráfico de dispersão e sua respectiva reta para identificar correlações. No caso presente usamos este gráfico para visualizar a relação entre sqrtfeet_above e preço, que nos deu uma correlação positiva,
entretanto, os dados foram ficando mais dispersos conforme os valores aumentaram, indicando uma limitação na capacidade preditiva.

Quarta Etapa: Modelagem
Para esta etapa, usamos o modelo de regressão linear múltipla, usando as seguintes variáveis de interesse: "floors", "waterfront","lat" ,"bedrooms" ,"sqft_basement" ,"view" ,"bathrooms","sqft_living15","sqft_above","grade",
"sqft_living". Preço foi a variável de resposta, obtendo um R2 de ~ 0.65, indicando que nosso modelo linear não é muito bom em prever o preço.
Uma maneira de tentar melhorar nosso modelo é a partir do que se chama 'pipeline', que melhora o desempenho por evitar que os dados teste vazem para o modelo de validação cruzada, ao garantir que a mesma amostra
é usada para treinar variáveis de interesse e resposta, obtendo um R2 de ~ 0.75, melhor do que nosso modelo linear original.

Após estes testes, testamos a regressão Ridge, com parametro de regularização em 0.1, tanto para o modelo linear e quadrático, obtendo respectivamente r2 de ~0.65 e 0.7, o que é esperado, uma vez que o propósito 
da regressão Ridge é desencorajar o ajuste excessivo de dados

Fontes:
https://www.toucantoco.com/en/blog/real-estate-kpis#:~:text=A%20real%20estate%20Key%20Performance,in%20the%20real%20estate%20industry.
https://medium.com/@kvmoura/crisp-dm-79580b0d3ac4#:~:text=O%20ciclo%20de%20vida%20apresentado,informa%C3%A7%C3%B5es%20ou%20aperfei%C3%A7oamentos%20do%20processo.
https://medium.com/turing-talks/turing-talks-20-regress%C3%A3o-de-ridge-e-lasso-a0fc467b5629
https://scikit-learn.org/stable/modules/compose.html


