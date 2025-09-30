# Equilíbrio de Nash em Leilões de Primeiro Preço: Uma Abordagem via Reinforcement Learning

#### Aluno: [Isabela Guerra Alvarez Guarino](https://github.com/isaguarino)
#### Orientadora: [Evelyn Batista](https://github.com/evysb)

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- Trabalhos relacionados: 
    - [Identificando as Causas do Fenômeno de Overbidding em Leilões de Primeiro Preço: Uma Análise Experimental](https://www2.dbd.puc-rio.br/pergamum/biblioteca/php/mostrateses.php?open=1&arqtese=0610512_08_Indice.html).

---

### Resumo

A proposta desse trabalho é criar um algoritmo de Reinforcement Learning que busque atingir o equilíbrio de Nash para leilões de primeiro preço com valorações privadas e independentes. A literatura de economia experimental mostra a existência do fenômeno de overbidding, quando os lances observados em experimentos realizados com pessoas são sistematicamente superiores ao equilíbrio de Nash ,previsto pela teoria econômica. O objetivo deste estudo é avaliar se um algoritmo computacional, livre dos vieses cognitivos humanos, consegue alcançar ou se aproximar significativamente desse equilíbrio teórico.

### 1. Introdução

Esse estudo analisa a capacidade de algoritmos de Reinforcement Learning em atingir o equilíbrio de Nash -  quando cada jogador escolhe a melhor ação possível (ótima) dada a estratégia do seu oponente- em leilões de primeiro preço com valorações privadas e independentes. Foram criados algoritmos como forma de avaliar dois cenários distintos. No primeiro, um único agente aprende jogando contra um oponente estático que segue sempre a estratégia de Nash. No outro formato de leilão, dois agentes aprendem simultaneamente e adaptam suas estratégias em resposta um ao outro. 

Os resultados demonstram uma boa capacidade dos agentes de aprender estratégias que se aproximam notavelmente do equilíbrio teórico, com desvios mínimos no caso multiagentes. 

### 2. Modelagem

O desenho dos leilões segue como referência os formatos dos experimentos conduzidos no estudo citado acima. Mais especificamente, um leilão de primeiro preço com valorações privadas e independentes consiste em jogadores que escolhem lances simultaneamente sem o conhecimento do outro jogador. O vencedor do leilão será o jogador que der o maior lance. Antes da escolha do lance , a cada episódio, cada jogador recebe a sua valoração. O jogador não tem a informação prévia da valoração do outro jogador, apenas conhece a sua distribuição de probabilidades (uniforme entre 0 e 1 , com precisão de 1 casa decimal). Após cada episódio do leilão, o jogador recebe as seguintes informações: quem venceu o leilão, qual foi o seu ganho (recompensa) e o lance do seu oponente. O objetivo de cada jogador é maximizar a sua utilidade (ganhos/recompensas) a cada episódio e ao longo do tempo. Seguindo o modelo em questão, iremos trabalhar com 2 diferentes tipos de leilão , ambos com 2 jogadores: (i) Aprendizado de um único agente: Um agente está em processo de aprendizado, enquanto o outro agente jogará sempre o equilíbrio de Nash (v/2) , estratégia conhecida pelo jogador aprendiz; (ii) Aprendizado simultâneo de dois agentes: Ambos agentes aprendem simultaneamente observando ao longo do tempo as ações de seu oponente e atualizando assim , as suas expectativas em relação a probabilidade de vencer o leilão. 

Em função da natureza distinta dos 2 modelos de leilão foram utilizados diferentes algoritmos para resolver cada um dos problemas apresentados. Para (i), um modelo de um unico agente, foi usado o método de Q-learning, mais especificamente Bandit Q-learning contextual, na medida em que os episódios são independentes , não havendo transição sequencial entre eles e que as ações do agente dependem de um estado contextual (valoração). A tabela Q é atualizada com taxa de aprendizado de 0.2 e a política exploração usada é e-greedy com epsilon decaindo exponencialmente de 1,0 até 0,02. Para se definir esses hiperparâmetros, foram feitos inúmeros testes ,  analisando a convergência das recompensas médias e o desvio dos lances aprendidos em relação ao equilíbrio de Nash e fazendo os ajustes necessários após cada simulação. Em (ii), devido a maior complexidade do modelo multiagente , foi utilizada uma abordagem híbrida, combinando o Q-learning com um modelo de aprendizado baseado na experiência da observação dos lances do oponente, para melhor inferir a sua estratégia e atualizar as estimativas de probabilidade de se vencer o leilão para cada possível ação (lance). Além disso, a estratégia de exploração foi alterada para Boltzmann , já que o e-greedy nao funcionou tão bem para aprendizado de 2 agentes, mostrando resultados mais distantes do equilíbrio de Nash.

<img width="837" height="271" alt="image" src="https://github.com/user-attachments/assets/fb813d60-d163-41a2-bbea-b0638c8d5608" />



### 3. Resultados

Os resultados do algoritmo do primeiro modelo de leilão, no qual o agente joga contra um oponente de Nash, demonstram que o agente foi capaz de aprender de forma consistente a estratégia de lances compatíveis com o equilíbrio de Nash. Após 6 milhões de episódios, o algoritmo convergiu para a política ótima (b = v/2), onde para cada estado (v), o agente aprendeu dar lances iguais a metade da sua valoração, como mostra a tabela abaixo. O grafico a seguir também mostra visualmente a comparação , para cada valoração, do lance aprendido com o equilíbrio teórico. A sobreposição das duas retas confirma que o aprendizado foi bem sucedido. Ainda, as recompensas médias, após convergência inicial mais rápida, estabilizaram-se em níveis consistentes com o equilíbrio (1.67).

<img width="440" height="401" alt="tabela1" src="https://github.com/user-attachments/assets/0cce2ab9-ad2f-49b0-bf14-c498641fdfa6" />


<img width="908" height="583" alt="graf1" src="https://github.com/user-attachments/assets/34ffb6a2-f5f2-447a-a088-b3380dc15c3c" />



No segundo formato de leilão, quando os 2 agentes aprendem simultaneamente, os resultados mostram um processo de convergência mais lento e com maiores flutuações em comparação ao cenário contra oponente de Nash. Ainda assim, após um número suficiente de episódios, observa-se que os lances aprendidos por ambos os agentes aproximam-se do equilíbrio teórico, embora apresentem desvios residuais em algumas valorações. Vale notar, contudo, que esses desvios são de baixa magnitude absoluta (em média 0.023) e não apresentam viés sistemático ,distinguindo-se do comportamento observado em experimentos com participantes humanos, nos quais é recorrente o fenômeno de overbidding. O gráfico e tabela abaixo ilustram a proximidade dos lances aprendidos em relação ao equilíbrio teórico, sugerindo um processo de aprendizado bem sucedido por parte dos agentes.



<img width="698" height="503" alt="tabela2 1" src="https://github.com/user-attachments/assets/c5f4345c-8d7c-4b29-b5a5-40bbd759254f" />



<img width="873" height="590" alt="graf2" src="https://github.com/user-attachments/assets/fdb79934-ca09-47bf-b152-78f3734dc675" />


### 4. Conclusões

Este trabalho demonstrou que algoritmos de Reinforcement Learning são capazes de reproduzir estratégias próximas ao equilíbrio de Nash em leilões de primeiro preço com valorações privadas e independentes. No cenário com oponente de Nash, a estratégia do agente convergiu exatamente para a solução teórica, enquanto que, no modelo multiagente de aprendizado simultâneo, os lances aprendidos apresentaram apenas pequenos desvios não enviesados em relação ao previsto pela teoria econômica.

---

Matrícula: 231.101.017

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*














