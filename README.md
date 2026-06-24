# 💳 Segmentação de Clientes de Cartão de Crédito (Aprendizado Não Supervisionado)

Este repositório contém o projeto final focado na análise do comportamento de clientes por meio da aplicação de algoritmos de **Aprendizado de Máquina Não Supervisionado** em dados transacionais de cartões de crédito. O estudo compara diferentes técnicas de *clustering* para segmentação de perfis de consumo e detecção de anomalias, oferecendo ferramentas acionáveis para o marketing e para a gestão de risco de crédito.

---

## 👥 Autores
Projeto desenvolvido por alunos do Instituto Metrópole Digital (IMD) da Universidade Federal do Rio Grande do Norte (UFRN):
* Clóvis L. M. Araújo
* Daniel V. O. Bezerra
* Pedro H. M. Xavier
* Weslley B. Oliveira

---

## 🛠️ Tecnologias e Bibliotecas Utilizadas
* **Linguagem:** Python
* **Manipulação de Dados:** Pandas, NumPy
* **Machine Learning:** Scikit-Learn (K-Means, AgglomerativeClustering, DBSCAN, PCA, t-SNE)
* **Visualização:** Matplotlib, Seaborn, SciPy (Dendrogramas)
* **Ambiente:** Jupyter Notebook / Google Colab

---

## 🗂️ Sobre a Base de Dados
O conjunto de dados utilizado é o **Credit Card Dataset for Clustering**, que documenta o comportamento de uso de 8.950 clientes ativos de cartões de crédito ao longo de um período de seis meses. A base conta originalmente com 18 variáveis comportamentais, incluindo saldo devedor, frequência e volume de compras, saques em dinheiro (*cash advance*) e taxas de pagamento.

---

## ⚙️ Metodologia e Pipeline Analítico

1. **Pré-processamento e Limpeza:**
   * Exclusão da variável categórica identificadora (`CUST_ID`).
   * Remoção direta de 314 registros nulos.
   * Padronização de escala de todas as variáveis contínuas utilizando o `StandardScaler`.

2. **Redução de Dimensionalidade:**
   * Aplicação da **Análise de Componentes Principais (PCA)** para mitigação da "Maldição da Dimensionalidade".
   * Retenção de **7 componentes principais**, definidos através da análise visual do *Scree Plot*, preservando mais de 80% da variância original.
   * Uso da técnica **t-SNE** para projeção e validação visual dos clusters formados.

3. **Modelagem Matemática (*Clustering*):**
   * **K-Means:** Parametrizado com $K=4$ (definido pelo Método do Cotovelo e validado pelo *Silhouette Score*).
   * **Agrupamento Hierárquico:** Abordagem aglomerativa usando a distância euclidiana e o critério de ligação de *Ward* (corte do dendrograma em $K=4$).
   * **DBSCAN:** Configurado para detecção de anomalias (ruído) com `min_samples=14` (dobro das dimensões) e $\epsilon = 3.0$ (determinado via gráfico *K-Distance*).

---

## 📊 Principais Resultados (Perfis de Negócio)

Os modelos K-Means e Hierárquico convergiram para a identificação de **4 segmentos comportamentais**:

1. 🛒 **Compradores Conscientes (Uso Cotidiano):** Clientes com baixo saldo devedor e uso razoável do cartão. Quase não sacam dinheiro em espécie e possuem uma alta taxa de liquidação total da fatura (baixo risco).
2. ⚠️ **Alto Risco (Endividados):** Compram pouco, mas possuem saldos devedores altíssimos devido ao volume extremo de saques em dinheiro. Pagam integralmente apenas 4% das faturas, vivendo no crédito rotativo.
3. 💤 **Inativos (Baixo Engajamento):** Clientes com limites baixos e atividade transacional mínima. Representam cartões "esquecidos", ideais para campanhas de reativação.
4. 💎 **VIPs (Alto Consumo):** Limites altíssimos e volume de compras exorbitante. Pagam as faturas em dia e geram alta rentabilidade. O grupo ideal para programas de fidelidade.

### 🚨 Auditoria e Prevenção a Fraudes (DBSCAN)
Enquanto a massa de dados foi identificada como normalidade (98,8%), o DBSCAN isolou com precisão **105 clientes anômalos (Cluster -1)**. Este grupo apresenta volumes financeiros que chegam a ser 11 vezes superiores à média de compras e 6 vezes superiores nos saques em dinheiro vivo, configurando alertas críticos para equipes de conformidade (*compliance*) e prevenção à lavagem de dinheiro.