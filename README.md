# 👾 Guia da Stable Baselines 

 Guia de como utilizar a biblioteca [Stable Baselines](https://github.com/hill-a/stable-baselines) para projetos de Aprendizagem por reforço.

O guia completo encontra-se neste notebook:

### [Guia Completo em Notebook](Guia%20da%20Stable%20Baselines.ipynb)

Entretanto, caso você não queira ver os exemplos em código, o texto do guia está presente a seguir.

## Índice

- [O que é Stable Baselines?](#o-que-é-stable-baselines?)
- [Instalação](#instalação)
- [Como usar Stable Baselines?](#Como-usar-Stable-Baselines?)
  - [Gym](#Gym)
  - [O que é um Ambiente?](#O-que-é-um-Ambiente?)
  - [Como Funciona um Ambiente do Gym?](#Como-Funciona-um-Ambiente-do-Gym?)
  - [Criando um Ambiente](#Criando-um-Ambiente)
  - [Criando um Agente](#Criando-um-Agente)

## O que é Stable Baselines?

A **Stable Baselines** é uma biblioteca de Aprendizagem por Reforço que implementa diversos algoritmos de agentes de RL, além de várias funcionalidades úteis para o treinamento de um agente. Suas implementações são bem simples e intuitivas, mas sem deixarem de ser otimizadas e poderosas, buscando facilitar o desenvolvimento de projetos de reforço de alta qualidade.

![Logo](https://github.com/hill-a/stable-baselines/raw/master/docs//_static/img/logo.png "Logo da Stable Baselines")

## Instalação

## Como usar Stable Baselines?

### Gym

O **[Gym](https://gym.openai.com/)** é uma biblioteca desenvolvida pela OpenAI que contém várias implementações prontas de ambientes de Aprendizagem por Reforço. Ela é muito utilizada quando se quer testar um algoritmo de agente sem ter o trabalho de programar seu próprio ambiente.

<img src="https://camo.githubusercontent.com/25043fb622d3f9115a263fb71c61adb08c1d7790/68747470733a2f2f7072617665656e702e636f6d2f70726f6a656374732f484f4941574f472f6f75747075742e676966" alt="Exemplos de Ambientes do Gym" class="inline"/>

### O que é um Ambiente?

Um **Ambiente** de Aprendizagem por Reforço é um espaço que representa o nosso problema, é o objeto com o qual o nosso agente deve interagir para cumprir sua função. Isso significa que o agente toma **ações** nesse ambiente, e recebe **recompensas** dele com base na qualidade de sua tomada de decisões.

Todos os ambientes são dotados de um **espaço de observações**, que é a forma pela qual o agente recebe informações e deve se basear para a tomada de decisões, e um **espaço de ações**, que especifica as ações possíveis do agente. No xadrez, por exemplo, o espaço de observações seria o conjunto de todas as configurações diferentes do tabuleiro, e o espaço de ações seria o conjunto de todos os movimentos permitidos.

<img src="https://www.raspberrypi.org/wp-content/uploads/2016/08/giphy-1-1.gif" alt="Uma Ação do Xadrez" class="inline"/>

### Como Funciona um Ambiente do Gym?

Agora que você já sabe o que é um ambiente, é preciso entender como nosso agente interage efetivamente com ele. Todos os ambientes do Gym possuem alguns métodos simples para facilitar a comunicação com eles:

<br>

| Método               | Funcionalidade                                          |
| :------------------- |:------------------------------------------------------- |
| reset()              | Inicializa o ambiente e recebe a observação inicial     |
| step(action)         | Executa uma ação e recebe a observação e a recompensa   |
| render()             | Renderiza o ambiente                                    |
| close()              | Fecha o ambiente                                        |

<br>

Assim, o código para interagir com o ambiente costuma seguir o seguinte modelo:

---

```python
ambiente = gym.make("Nome do Ambiente")                         # Cria o ambiente
observação = ambiente.reset()                                   # Inicializa o ambiente
acabou = False

while not acabou:
    ambiente.render()                                           # Renderiza o ambiente
    observação, recompensa, acabou, info = ambiente.step(ação)  # Executa uma ação
    
ambiente.close()                                                # Fecha o ambiente
```

---

### Criando um Ambiente

Para utilizar um dos ambientes do Gym, nós utilizamos a função ```gym.make()```, passando o nome do ambiente desejado como parâmetro e guardando seu valor retornado em uma variável que chamaramos de ```env```. A lista com todos os ambiente pode ser encontrada [aqui](https://gym.openai.com/envs/#classic_control).

#### Exemplo - CartPole

Para exemplificar, vamos pensar no ambiente ```CartPole-v1```, um ambiente bem simples que modela um pêndulo invertido em cima de um carrinho buscando seu estado de equilíbrio.

<img src="https://miro.medium.com/max/1200/1*jLj9SYWI7e6RElIsI3DFjg.gif" width="400px" alt="Ambiente do CartPole-v1" class="inline"/>

Antes de treinar qualquer agente, primeiro é preciso entender melhor quais as características do nosso ambiente.

O **Espaço de Observação** do CartPole é definido por 4 informações:

<br>

|     | Informação                         | Min     | Max    |
| :-- | :--------------------------------- | :-----: | :----: |
| 0   | Posição do Carrinho                | -4.8    | 4.8    |
| 1   | Velocidade do Carrinho             | -Inf    | Inf    |
| 2   | Ângulo da Barra                    | -24 deg | 24 deg |
| 3   | Velocidade na Extremidade da Barra | -Inf    | Inf    |

<br>

Dessa forma, a cada instante recebemos uma lista da observação com o seguinte formato:

```[-3.3715708e+00 -2.6997593e+38  2.7833584e-01  2.0276438e+38]```

Já o **Espaço de Ação** é composto por duas ações únicas: mover o carrinho para a **esquerda** ou para a **direita**.

Quando queremos mover o carrinho para a esquerda, fazemos um `env.step(0)`; quando queremos movê-lo para a direita, enviamos um `env.step(1)`