# MVP — Análise de Dados: Selic, IPCA e dólar (séries do Banco Central)

**Autor:** Matheus Cecatto Paim
**Disciplina:** Análise de Dados e Boas Práticas
**Tipo de trabalho:** análise exploratória e pré-processamento (sem modelo preditivo)

## Contexto
Selic, inflação e câmbio afetam o custo de crédito, os custos operacionais e as decisões de investimento de empresas como uma transportadora. O trabalho usa séries públicas do Banco Central para olhar os três indicadores juntos, com um mês como unidade de observação.

## Perguntas
- **H1:** a Selic e a inflação têm relação inversa, mas com defasagem? Testadas defasagens de 0 a 24 meses.
- **H2:** a volatilidade do dólar é maior em períodos de instabilidade (2015, 2020, 2022)? Comparada a volatilidade anual.

## Dados
| Série | Código SGS | Unidade | Registros |
|---|---|---|---|
| IPCA mensal | 433 | % a.m. | 202 |
| Selic acumulada no mês | 4390 | % a.m. | 202 |
| Dólar (venda, diário) | 1 | R$/US$ | 4.227 |

- **Fonte:** Banco Central do Brasil, API SGS (https://api.bcb.gov.br), dados públicos sem chave.
- **Período:** 01/2010 a 10/2026. Snapshot baixado em 16/11/2026 e guardado em `bcb_sgs_bruto.csv`, para que o notebook rode sempre com os mesmos dados.

## Estratégia
1. Diagnóstico de qualidade: nulos, duplicatas, lacunas de datas e extremos (IQR).
2. Estatísticas descritivas e três figuras (séries de IPCA e Selic, dólar com volatilidade, correlação defasada).
3. Decisões documentadas:
   - **D1:** dólar diário agregado em mês pela média, comparada com o fim do mês.
   - **D2:** manter os valores extremos, por serem eventos reais.
   - **D3:** usar retorno logarítmico em vez do nível para medir volatilidade, comparado com a variação percentual.
4. Divisão treino/teste e vazamento: não se aplica (trabalho descritivo); justificativa no notebook.

## Principais achados
- **H1 apoiada em parte:** Selic média de 0,36% em 2021 e 0,98% em 2022, com IPCA médio de 0,80% e 0,47%. Correlação de −0,26 em torno de 12 meses, fraca a moderada, sem indicar causa.
- **H2 apoiada em parte:** volatilidade anualizada de 0,1915 (2015) e 0,2027 (2020). 2022 (0,1608) ficou abaixo de 2016 (0,1690), então a volatilidade alta não ficou restrita aos anos esperados.
- O pico de volatilidade no fim de out/2026 vem de um único dia (dólar de R$ 5,0377 para R$ 5,5007 entre 23 e 26/10).

## Limitações
Selic acumulada (efetiva) em vez da meta do Copom, dependente dos dias úteis; correlação em nível; sem outras variáveis; 2026 incompleto.

## Como executar
1. Abra `mvp3_bcb.ipynb` no Google Colab.
2. Menu **Ambiente de execução → Executar tudo**.
3. O notebook lê `bcb_sgs_bruto.csv` direto deste repositório; não é preciso upload, conta ou chave. Tempo de execução: menos de 1 minuto.

## Arquivos
- `mvp3_bcb.ipynb` — notebook do trabalho
- `bcb_sgs_bruto.csv` — snapshot dos dados brutos
- `README.md` — este arquivo
