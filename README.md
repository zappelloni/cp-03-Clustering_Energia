# Checkpoint 3 — Clustering aplicado à energia

Agrupamento de unidades consumidoras de energia com **K-Means**, usando o **método Elbow** e o **Silhouette Score** para escolher o número de clusters.

## Integrantes

| Nome | RM |
| --- | --- |
| Gabriel Rodrigues Zappelloni | 572060 |

## Objetivo

Uma equipe de gestão de energia quer conhecer os diferentes perfis de uso das suas unidades consumidoras para planejar ações de acompanhamento e uso consciente. O objetivo da atividade é:

1. explorar e padronizar os dados;
2. aplicar o K-Means e comparar diferentes valores de **k** com o método Elbow e o Silhouette Score;
3. justificar a escolha de k;
4. visualizar e interpretar os perfis encontrados;
5. propor uma ação de gestão de energia para cada perfil e discutir as limitações da análise.

## Base de dados

Base **simulada**, com **120 unidades consumidoras fictícias** de um mesmo mês de referência. Os dados foram gerados com NumPy (semente 303) para fins didáticos e estão **incorporados no próprio notebook** como texto CSV, então não é preciso baixar nenhum arquivo. Eles não vieram de medidores, empresas nem bases públicas.

| Coluna | Significado | Unidade / uso |
| --- | --- | --- |
| `id_unidade` | Identificador da unidade consumidora | Identificação; não entra no modelo |
| `consumo_mensal_kwh` | Energia total consumida no mês | kWh |
| `demanda_maxima_kw` | Maior demanda de potência média em uma janela de 15 minutos no mês | kW |
| `consumo_noturno_pct` | Parcela da energia mensal consumida entre 18h e 6h | % (0 a 100) |

A base não tem valores ausentes nem identificadores repetidos.

## Metodologia

1. **Exploração:** formato, tipos, valores ausentes, duplicados e estatísticas descritivas.
2. **Padronização:** as três variáveis numéricas foram padronizadas com `StandardScaler` (média 0, desvio padrão 1), porque o consumo, em centenas de kWh, dominaria as distâncias do K-Means.
3. **Elbow:** inércia do K-Means para k de 1 a 8.
4. **Silhouette Score:** score médio para k de 2 a 8.
5. **Modelo final:** K-Means com o k escolhido; os rótulos foram adicionados em uma cópia do DataFrame (`df_resultados`), sem entrar nas variáveis do modelo.
6. **Interpretação:** gráficos de dispersão nas unidades originais e tabela de médias por cluster.

## Principais resultados

**Escolha de k = 3:** as duas métricas apontam para o mesmo valor.

| k | Inércia | Silhouette Score |
| ---: | ---: | ---: |
| 1 | 360,00 | — |
| 2 | 184,70 | 0,4835 |
| **3** | **74,25** | **0,6031** |
| 4 | 61,10 | 0,5190 |
| 5 | 52,06 | 0,4282 |
| 6 | 46,36 | 0,4343 |
| 7 | 40,90 | 0,3240 |
| 8 | 37,25 | 0,3106 |

- **Elbow:** a inércia cai bastante até k = 3 (de 184,70 para 74,25) e, a partir daí, cada cluster extra reduz no máximo 13,15. Ou seja, k = 3 é o cotovelo.
- **Silhouette:** o maior score do intervalo é **0,6031, com k = 3**.

**Perfis encontrados (40 unidades em cada grupo):**

| Cluster | Perfil | Consumo médio | Demanda média | Consumo noturno | Ação proposta |
| --- | --- | ---: | ---: | ---: | --- |
| 0 | Consumo médio, uso diurno e picos de potência | 876,50 kWh | 6,87 kW | 25,49% | Escalonar cargas grandes para reduzir os picos de demanda |
| 1 | Baixo consumo | 245,09 kWh | 2,58 kW | 60,82% | Acompanhamento simples e alertas de aumento de consumo |
| 2 | Alto consumo, concentrado à noite | 1.433,93 kWh | 6,15 kW | 67,37% | Prioridade de auditoria para identificar as cargas noturnas |

**Limitações:** a base é simulada e cobre só um mês e três variáveis. Consumo alto não prova desperdício, e recomendações sobre tarifa ou geração solar exigiriam dados de contrato, horários, local e viabilidade. O K-Means também exige escolher k antes e supõe grupos de formato aproximadamente esférico.

A análise completa, com os gráficos, está no notebook.

## Arquivos

| Arquivo | Conteúdo |
| --- | --- |
| `Checkpoint_3_Clustering_Energia.ipynb` | Notebook preenchido e executado: códigos, tabelas, gráficos, análises e conclusões |
| `README.md` | Este arquivo |
| `requirements.txt` | Bibliotecas necessárias para executar o notebook |
