# Trainee CIS - 2º Período
## Classificação Estelar (SDSS17): 
### 1. Introdução
Neste trabalho, é detalhado o desenvolvimento de uma Rede Neural Artificial (RNA) utilizando PyTorch, com o propósito de distinguir corpos celestes em três classes: Galáxias, Estrelas e Quasares (QSO). As informações utilizadas têm origem no Stellar Classification Dataset (SDSS17). O principal ponto de análise foi o modo como a estrutura da rede (sua largura e profundidade) e o uso de métodos de regularização influenciaram a capacidade de o modelo se aplicar corretamente a dados não vistos (generalização).

### 2. Preparação e Pré-processamento dos Dados
Antes de se definir a estrutura da rede, foi feita uma preparação dos dados. Isso foi essencial para assegurar a consistência no cálculo dos gradientes:

- **Análise de Distribuição:** Foi verificado um desequilíbrio esperado nos dados (~60% Galáxias, ~21% Estrelas, ~19% Quasares).
- **Remoção de Elementos Não Físicos:** Os dados que eram apenas metadados do telescópio (como run_ID, cam_col, etc.) foram retirados. Assim, ficaram apenas as informações físicas relevantes (bandas ópticas u, g, r, i, z e o redshift).
- **Ajuste de Falhas:** Um erro de registro do telescópio (-9999.00) foi identificado em uma observação, e a linha completa foi removida do conjunto de dados.
- **Normalização:** As variáveis de previsão foram ajustadas com o StandardScaler (Z-score), e a variável de classificação foi transformada em números inteiros (LabelEncoder).

<img width="731" height="476" alt="image" src="https://github.com/user-attachments/assets/6cf032f9-ba9f-441e-ae20-9df2066cd305" />

### 3. O Modelo Inicial (Baseline) e a Avaliação de Desempenho
A primeira estrutura de rede a ser testada (StarClassifierBase) tinha uma composição básica:
- **Camada Inicial:** Foi configurada com 6 neurônios.**
- **Camadas Intermediarias:** Foram utilizadas duas camadas (com 32 e 16 neurônios, respectivamente), usando a função de ativação ReLU.
- **Camada Final:** Definida com 3 neurônios.
- **Processo:** Foi usada a função de perda CrossEntropyLoss e o otimizador Adam (com Learning Rate de 0.001) ao longo de 30 ciclos (épocas).
- **Resultados Iniciais:** O estado de bom ajuste (Good Fit) foi alcançado rapidamente. Observou-se que a curva de erro (Loss) do treino se manteve em harmonia com a curva de validação/teste. Não se verificaram sinais de aprendizagem insuficiente (Underfitting, os padrões complexos foram assimilados pela rede) nem de memorização excessiva (Overfitting) repentina, o que resultou numa precisão (accuracy) de cerca de 97.2%.

  <img width="852" height="471" alt="image" src="https://github.com/user-attachments/assets/f6ea66ac-1673-44a4-9622-ca3f667c4952" />


### 4. O Modelo Otimizado: Estrutura e Regularização
Para levar o projeto mais além, a rede foi ampliada para se avaliar o impacto de um maior poder de processamento:
- **Nova Estrutura:** A rede foi definida como mais ampla e mais profunda (128 -> 64 -> 32 neurônios nas camadas intermediárias).
- **Controle de Memorização:** Por se saber que um grande aumento de parâmetros numa rede profunda pode levar à memorização dos dados (Overfitting), foi introduzida a técnica de Dropout (30%). Por meio deste método, é feito um "desligamento" aleatório de uma percentagem dos neurônios durante o treino, o que obriga o sistema a encontrar várias rotas de aprendizagem para chegar à mesma conclusão astrofísica.
- **Resultados Otimizados:** Foi alcançada uma precisão de 96.81%. Apesar de este valor ser marginalmente mais baixo que o do modelo inicial, a arquitetura final mostrou-se consideravelmente mais estável. O Dropout teve o efeito desejado: a rede foi impedida de memorizar o conjunto de treino, o que assegurou uma maior fiabilidade na classificação de futuros dados espaciais.

### 5. Diagnóstico dos Erros (Análise da Matriz de Confusão)
A avaliação foi além da percentagem total de acertos. A análise da Matriz de Confusão permitiu ver o comportamento da rede em termos de física estelar:

O desempenho na identificação de Estrelas foi quase irrepreensível, com apenas 3 erros de classificação em mais de 4300 amostras.

A maior parte dos erros está na separação entre Quasares e Galáxias (243 Quasares foram classificados incorretamente como Galáxias).

**Justificativa:** Este não é um erro grave do algoritmo. Em vez disso, é um resultado da própria complexidade do universo. Quasares são, na verdade, os centros ativos e muito brilhantes de galáxias longínquas, apresentando uma assinatura eletromagnética que é frequentemente confundida até por instrumentos ópticos avançados. A ambiguidade inerente a estes objetos cósmicos foi capturada de maneira eficiente pelo modelo.

<img width="1554" height="582" alt="image" src="https://github.com/user-attachments/assets/38770bb1-4bca-4a36-89d4-6e5995cead1b" />

### 6. Conclusão
Com este projeto, confirmou-se a capacidade da plataforma PyTorch para resolver tarefas complexas de classificação em múltiplas categorias. Foi demonstrada a importância da preparação precisa dos tensores, da definição gradual da estrutura (mais larga vs. mais profunda) e do uso estratégico do Dropout. 

