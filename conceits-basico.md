# Conceitos Básico de Reinforcement Learning
O *Reinforcement Learning* (RL) - Aprendizado por Reforço - é o problema de estudar um agente em um ambiente, onde
o agente precisa interagir com o ambiente para maximizar algumas recompensas cumulativas, aprendendo dentro de um ciclo de tentativa e erro
as melhores ações.

![img-rf](https://www.oreilly.com/radar/wp-content/uploads/sites/3/2019/06/image3-5f8cbb1fb6fb9132fef76b13b8687bfc.png)

Exemplo: um agente em um labirinto tentando encontrar sua saída. Quanto mais rápido ele encontrar a saída, melhor será a recompensa.

## Markov Decision Process (MDP)
MDP é uma coleção de estados, ações, probabilidades de transição, recompensas e fator de desconto: (***S***, ***A***, ***P***, ***R***, ***γ***)
- ***S*** é um conjunto de um estado finito que descreve o ambiente
- ***A*** é um conjunto de ações finitas que descreve a ação que pode ser executada pelo agente
- ***P*** é uma matriz de probabilidade que indica a probabilidade de se mover de um estado para outro
- ***R*** é um conjunto de recompensas que dependem do estado e das medidas tomadas. 
As recompensas não são necessariamente positivas, elas devem ser vistas como resultado de uma ação realizada pelo agente quando ela está em um determinado estado. 
Portanto, recompensa negativa indica mau resultado, enquanto recompensa positiva indica bom resultado
- ***γ*** é um fator de desconto, que indica a importância das recompensas futuras para o estado atual. O fator de desconto é um valor entre 0 e 1. 
Uma recompensa R que ocorrer N etapas no futuro a partir do estado atual é multiplicada por γ ^ N para descrever sua importância para o estado atual. Por exemplo, considere γ = 0,9 e uma recompensa R = 10 que está 3 passos à frente do nosso estado atual. 
A importância dessa recompensa para nós de onde estamos é igual a (0,9³) * 10 = 7,29.

# Funções de valor
Como o agente deve agir nesse ambiente?
A regra que impomos ao agente é que ele deve agir de maneira a maximizar as recompensas coletadas.
Para poder fazer isso, o agente deve ter algo para estimar a posição ou estado em que está atualmente. Considere novamente o labirinto: se o agente estiver a poucos passos da saída, sua posição terá muito mais valor do que uma posição na centro do labirinto.
Chamamos essa estimativa de Valor da posição ou Valor do estado. Uma vez que conhecemos o valor de cada estado, podemos descobrir qual é a melhor maneira de agir (simplesmente seguindo o estado com o valor mais alto).
Ainda precisamos descobrir uma maneira de quantificar o valor de cada estado. Isso é feito por uma função chamada Sate-Value Function:

![img-svfunction](https://miro.medium.com/max/531/1*NNCFu-9jq25GKTaDXdUzjQ.png)
[mais informações sobre a equação](https://towardsdatascience.com/math-behind-reinforcement-learning-the-easy-way-1b7ed0c030f4)

O valor de cada estado é a média das recompensas futuras com desconto,
***v(s)*** é expresso em termos de recompensas imediatas ***r*** valor dos estados vizinhos ***v(s')***

### Interpretações da equação
- quando esta num estado ***s***, considere todas as ações possíveis;
- para cada ação ***a*** obtenha a probabilidade ***p(s', r | s, a)*** de receber recompensa imediata ***r*** e mudar para o estado vizinho ***s'***, 
sabendo que estamos no estado ***s*** e realizamos a ação ***a***;
- adicione a recompensa ***r*** ao valor descontado do estado vizinho ***s'*** dado por ***γ * v (s')***. 
Multiplique o resultado por ***p (s ', r | s, a)*** e obtemos ***p (s', r | s, a) * [r + γ * v (s ')]***;
- Repita as etapas 2 e 3 para todos os estados ***s'*** vizinhos ***s***, bem como todas as recompensas possíveis ***r*** e calcule a soma dos resultados. 
Agora temos ***Sum (p (s ', r | s, a) * [r + γ * v (s')])*** sobre todos os ***s'*** e ***r***;
- Isso fornecerá a média das recompensas futuras com desconto ao executar apenas uma ação ***a***. 
No entanto, existem várias ações a serem tomadas, portanto, precisamos calcular a média de todas as ações. 
Para fazer isso, multiplicamos pela probabilidade ***𝜋 (a | s)*** de que a ação ***a*** seja executada no estado ***s***, 
e somamos todas as ações possíveis nesse estado.

*(quando estamos em um estado, temos uma probabilidade de tomar alguma ação que pode nos levar a diferentes estados com diferentes recompensas. 
Portanto, o valor do nosso estado é a média de todas as recompensas com descontos que poderíamos receber ao executar todas as ações possíveis no estado atual.)*

 Cálculo para obter um número que represente o valor do atual estado.
 
 ***v(s)*** diz como é bom estar no estado ***s***, mas não diz como é bom executar a ação ***a*** por um tempo no estado ***s***. 
 Este é o objetivo da função Action-Value:
 
 ![img-avfunction](https://miro.medium.com/max/344/1*wYxr2uTHTqtkTQ5wneDWsg.png)
 
 Interpretação: tomamos ***v(s)*** e, em vez de executar todas as ações, decidimos executar apenas uma ação. 
 Não nos perguntamos mais qual é a probabilidade de usar a ação sobre todas as ações possíveis, mas dizemos que usaremos a ação ***a***. 
 Portanto, a soma de ***𝜋 (a | s)*** não se aplica mais. Com essa lógica, reduzimos ***v(s)*** da média de todas as ações possíveis para simplesmente 
 usar uma ação selecionada, denotamos isso como ***q (s, a)***.
 
 Observe que ***q(s, a)*** verifica o valor da ação no estado ***s*** enquanto os valores ***v(s')*** dos estados vizinhos são mantidos inalterados. 
 Portanto, nesse sentido, verifica como essa ação é boa no estado atual, mantendo todo o resto igual.


Também é fácil perceber que ***v(s)*** é a média ponderada de ***q(s, a)*** em todas as ações possíveis no estado ***s***.

![img](https://miro.medium.com/max/229/1*LyWMoIu2JtC5GaAqIS9T3A.png)
