# Otimização de Carteiras de Investimento - Trabalho Final de PO

Este repositório contém o código fonte e os dados utilizados no Trabalho Final da disciplina de Introdução à Pesquisa Operacional. O objetivo do projeto é resolver o problema de seleção e otimização de carteiras (baseado no modelo média-variância de Markowitz com restrições de cardinalidade), comparando duas abordagens:

1.  **Método Exato:** Utilizando modelação matemática com AMPL e solver Gurobi.
2.  **Método Heurístico:** Utilizando um Algoritmo Genético de Chaves Aleatórias Viciadas (BRKGA).

## 📂 Estrutura do Repositório

O projeto está organizado da seguinte forma:

  * **`Limpa dados.py`**: Script responsável pelo pré-processamento dos dados históricos. Lê os preços de fecho, limpa ativos com dados insuficientes, e calcula o vetor de retornos esperados ($\mu$) e a matriz de covariância ($\Sigma$).
  * **`gerar_classificacao.py`**: Script que realiza *web scraping* da Wikipedia para obter a classificação setorial (GICS) das empresas do S\&P 500.
  * **`Implementacao_Exata`**: Código fonte para a resolução do problema via método exato (requer AMPL/Gurobi). Gera a fronteira eficiente ótima.
  * **`Implementacao_Heuristica`**: Código fonte da meta-heurística BRKGA. Procura soluções aproximadas e compara os resultados com o modelo exato.
  * **`Resultados`**: Ficheiro que indica a localização dos resultados finais (armazenados externamente/Google Drive).
  * **Ficheiros de Dados (`.csv`):**
      * `precos_fechamento_3_anos.csv`: Dados brutos históricos.
      * `vetor_retornos_calculado.csv`: Retornos médios anuais por ativo (Output do `Limpa dados.py`).
      * `matriz_covariancia_calculada.csv`: Risco/Covariância entre ativos (Output do `Limpa dados.py`).
      * `mapeamento_setores.csv`: Setor de cada empresa (Output do `gerar_classificacao.py`).

## 🚀 Pré-requisitos

Para executar este projeto, é necessário o **Python 3.8** e das seguintes bibliotecas:

```bash
pip install pandas numpy matplotlib seaborn requests amplpy scipy
```

> **Nota:** Para a `Implementacao_Exata`, é necessário ter o software **AMPL** e um solver compatível (ex: **Gurobi** ou CPLEX) instalados e configurados no sistema.

## ⚙️ Como Executar

A execução deve seguir uma ordem lógica para garantir que os dados necessários foram gerados:

### 1\. Preparação dos Dados

Primeiro, gera-se a classificação dos setores e processam-se os dados históricos:

```bash
# 1. Obter setores das empresas
python gerar_classificacao.py

# 2. Calcular retornos e covariância
python "Limpa dados.py"
```

### 2\. Execução da Otimização Exata

O código da implementação exata utiliza a biblioteca `amplpy`.

  * **Atenção:** Verifica a classe `Config` no ficheiro, pois os caminhos para o sistema AMPL (`PATH_AMPL_SYSTEM`) e para os dados (`BASE_PATH_DADOS`) podem precisar de ser ajustados para o teu diretório local.

### 3\. Execução da Otimização Heurística

A heurística BRKGA utilizará os ficheiros gerados (`vetor_retornos_calculado.csv`, etc.) e, opcionalmente, o ficheiro de resultados do modelo exato para gerar gráficos comparativos.

## 📊 Metodologia

O problema visa maximizar o retorno da carteira sujeito a um limite de risco, ou minimizar o risco para um dado retorno alvo, respeitando:

  * **Restrição de Cardinalidade:** A carteira deve conter um número fixo de ativos ($K$).
  * **Limites de Peso:** Cada ativo escolhido deve ter um peso entre $w_{min}$ e $w_{max}$.
  * **Restrições Setoriais:** (Se aplicável conforme o código).

Os resultados mostram a comparação entre a Fronteira Eficiente "verdadeira" (Exata) e a aproximada (Heurística).

## 📝 Autores

  * Rafael Pires Moreira Silva e Maycon Prado


1.  **Caminhos Relativos:** Nos ficheiros de implementação (como visto nos excertos), os caminhos estão absolutos (ex: `C:\Users\Cliente\...`). Sugiro alterá-los para caminhos relativos ou usar a biblioteca `os.path` para garantir que o código funcione em qualquer computador sem edições manuais.
2.  **Requirements:** Cria um ficheiro `requirements.txt` para facilitar a instalação das dependências.
