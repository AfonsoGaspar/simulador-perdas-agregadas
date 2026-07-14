# Simulador de Perdas Agregadas 

Este projeto implementa um **simulador estocástico** para modelar e avaliar o perfil de perdas agregadas de uma carteira de seguros anual. 

O objetivo principal é analisar como a escolha da distribuição de probabilidade da severidade dos sinistros influencia o cálculo de métricas de risco essenciais como o *Value at Risk* (*VaR*) a 99.5%, *Expected Shortfall* e, consequentemente, o Capital Económico.

Neste projeto, o Capital Económico é definido como a diferença entre o *VaR* a 99.5% e a perda esperada, sendo assim uma métrica que reflete perdas inesperadas. 

---

##  Lógica do Modelo

O montante total que uma seguradora terá de pagar num ano ($S$) é modelado como uma **soma aleatória de variáveis aleatórias**:

$$S = \sum_{i=1}^{N} X_i$$

A simulação de cada ano decorre em três etapas:
1. **Frequência ($N$):** O número de sinistros ocorridos no ano é gerado a partir de uma distribuição de **Poisson** com parâmetro $\lambda = 5$.
2. **Severidade ($X_i$):** Para cada um dos $N$ sinistros, o custo individual é gerado de forma independente de acordo com uma das três distribuições sob análise:
   * **Gama** 
   * **Lognormal** 
   * **Pareto** 
3. **Perda Agregada ($S$):** Corresponde à soma dos custos dos $N$ sinistros gerados.

Este processo é repetido **10 000 vezes** para cada distribuição de severidade de forma a construir as distribuições empíricas de $S$.

---

##  Parametrização das Distribuições

Para garantir uma comparação justa e consistente, os parâmetros das três distribuições de severidade foram calibrados para que todas partilhem exatamente a **mesma** média e a **mesma** variância teórica:

* **Média ($E[X]$):** $5.000$
* **Variância ($Var[X]$):** $25.000.000$

### Fórmulas e Parâmetros Obtidos:

| Distribuição | Média ($E[X]$) | Variância ($Var[X]$) | Parâmetros Calibrados |
| :--- | :--- | :--- | :--- |
| **Gama $(\alpha, \theta)$** | $\alpha \cdot \theta$ | $\alpha \cdot \theta^2$ | $\alpha = 1$ <br> $\theta = 5000$ |
| **Lognormal $(\mu, \sigma)$** | $\exp\left(\mu + \frac{\sigma^2}{2}\right)$ | $\exp(2\mu + \sigma^2) \cdot (\exp(\sigma^2) - 1)$ | $\mu = 8.1706$ <br> $\sigma = 0.8326$ |
| **Pareto $(x_m, \beta)$** | $\frac{\beta \cdot x_m}{\beta - 1}$ | $\frac{x_m^2 \cdot \beta}{(\beta - 1)^2 \cdot (\beta - 2)}$ | $\beta = 2.4142$ <br> $x_m = 2928.93$ |

---

## Tecnologias e Bibliotecas Utilizadas

O projeto foi inteiramente desenvolvido em **Python** (usando *Jupyter Notebooks*) recorrendo às seguintes bibliotecas base:

* **NumPy:** Para a geração estocástica de números pseudo-aleatórios e manipulação eficiente de arrays.
* **Matplotlib & Seaborn:** Para a construção e visualização dos histogramas das perdas agregadas.
* **IPython:** Para renderização de outputs otimizados no ambiente interativo.

---

##  Como Executar o Projeto

1. **Clona este repositório:**
   ```bash
   git clone https://github.com/AfonsoGaspar/simulador-perdas-agregadas.git
   cd simulador-perdas-agregadas