## 1) Descreva com suas palavras os principais conceitos abaixo:

a) O que é um Data Warehouse?
- R: Data Warehouse é um sistema de armazenamento centralizado orientado a análise de dados, que integra dados históricos de múltiplas fontes, otimizando consultas complexas e relatórios gerenciais.

b) Quais características possuem as tabelas do tipo Fato e Dimensão?
- R: Tabela Fato registra métricas quantitativas (ex: vendas, valores), enquanto Dimensão armazena atributos descritivos (ex: tempo, produto, local) usados para filtrar e agrupar análises.

c) O que é ETL?
- R: ETL é o processo automatizado de Extrair dados, Transformar (limpar, integrar, enriquecer) e Carregar em sistemas analíticos, como Data Warehouses.

d) Quais são as principais atribuições de um Engenheiro de Dados?
- R: Projetar pipelines de dados, modelar bases, garantir qualidade, governança e integração entre sistemas para suportar análises de negócio.

e) O que é Trade Marketing?
- R: Trade Marketing são ações para destacar produtos no ponto de venda, aumentar vendas e fortalecer a relação entre indústria e varejo.

---

## 2) Query MySQL para exibir pontos de venda com sell in > 20.000 ordenados por nome

```sql
SELECT
    *
FROM
    PONTO_VENDA_UNIDADE
WHERE
    SELLIN > 20_000
ORDER BY
    NOME_PDV
```

---

## 3) Query para somar sell in por ponto de venda, mês e ano

```sql
SELECT
    NOME_PDV,
    CONCAT(MES, '/', ANO) as MONTH_TRUNCD,
    SUM(SELLIN)
FROM
    PONTO_VENDA_UNIDADE
GROUP BY
    1,2
ORDER BY
    2,1
```

---

## 4) Query para calcular visitas do ponto de venda INVOLVES

```sql
with visitas_agregadas as (
    select
        FK_PDV,
        sum(FL_VISITADO) as total_visitas
    from
        VISITAS_PONTO_VENDA
    group By
        FK_PDV
)
SELECT 
    NOME_PDV,
    total_visitas
FROM
    PONTO_VENDA_UNIDADE
    join visitas_agregadas on visitas_agregadas.FK_PDV = PONTO_VENDA_UNIDADE.ID_PDV
WHERE
    NOME_PDV = 'INVOLVES'
```

---

## 5) Índices recomendados para melhorar performance da query

**R:**
Para melhorar a performance da query, os campos que participam de joins e filtros devem ser indexados. Recomendo criar índices nos seguintes campos:

- FT_DOMINANCIA_PONTO_EXTRA_COMPLIANCE: CICLO, ID_DIM_PDV, ID_BLOCO_ITEM, SEMANA_LP
- TABREF_PAINEL_LOJAS_LP: ID_DIM_PDV, CICLO
- FT_PERFECTSTORE_ITEM: CICLO, ID_DIM_PDV, ID_BLOCO_ITEM, SEMANA_LP
Esses índices vão acelerar as operações de junção (JOIN) e o filtro pelo campo CICLO no WHERE.

---

## 6) Execução da instrução Python

```python
x = [ print(i) for i in range(10) if i % 2 == 0 ]
```

### **R:** A variável `x`, será uma lista dos números pares entre 0 e 10.

---

## 7) Script Python para somar dois números

```python
nums = []
while len(nums) < 2:
    num = float(input('Put a number to sum: '))
    nums.append(num)
print(sum(nums))
```

---

## 8) Questões Pentaho Data Integration (PDI)

Para responder às questões 8, 9 e 10 utilize a ferramenta Pentaho Data Integration (PDI) na versão de sua preferência. A ETL final deve conter um job principal que, por sua vez, deve conter as transformações criadas nas questões 8, 9, 10. Além disso, que tal ganhar um ponto a mais nessas questões? Para isso, inclua o projeto criado em um repositório do Github (é importante que seja público para termos visibilidade, ok?). Compartilhe por aqui o link para o repositório.

### a) Dimensão Calendário (DIM_CALENDARIO): Deve conter data, mês e ano da coleta
### b) Dimensão Ponto de Venda (DIM_PDV): Deve conter o id, nome e perfil do ponto de venda
### c) Dimensão Linha de Produto (DIM_LINHA_PRODUTO): Deve conter o id, nome e perfil da linha de produto

- [DIM_CALENDARIO](./Transformations/DIM_CALENDAR.ktr)
- [DIM_PDV](./Transformations/DIM_PDV.ktr)
- [DIM_LINHA_PRODUTO](./Transformations/DIM_LINHA_PRODUTO.ktr)

---

## 9) Transformação usando DATASET_TESTE_DE.csv

A transformação deve consultar o dataset e inserir, em uma base de dados (modelo dimensional), as informações coletadas, conforme as tabelas abaixo:

### a) Fato Disponibilidade (FT_DISPONIBILIDADE): Deve conter os ids de ligação das tabelas de dimensões criadas na questão anterior e a quantidade de presenças de cada linha de produto no mês de Setembro/20.
### b) Fato Disponibilidade Agregada (FT_DISPONIBILIDADE_AGREGADA): Deve conter os ids de ligação das tabelas de dimensões (Dimensão Calendário e Ponto de Venda) e a quantidade de presença de linhas de produto agrupadas por ponto de venda no mês de Setembro/20.

Obs: Os dados de “Disponibilidade” estão categorizados na coluna TIPO_COLETA com o valor “Disponibilidade”. A presença é contada sempre que no campo VALOR aparecer o valor “SIM”

- [FT_DISPONIBILIDADE](./Transformations/FT_DISPONIBLIDADE.ktr)
- [FT_DISPONIBILIDADE_AGREGADA](./Transformations/FT_DISPONIBLIDADE_AGREGADA.ktr)

---

## 10) Transformação usando DATASET_TESTE_DE.csv

A transformação deve consultar o dataset e inserir, em uma base de dados (modelo dimensional), as informações coletadas, conforme as tabelas abaixo:

### a) Fato Ponto Extra (FT_PONTO_EXTRA): Deve conter os ids de ligação das tabelas de dimensões criadas na questão anterior e a soma de ponto extras de cada linha de produto no mês de Setembro/20.
### b) Fato Ponto Extra Agregada (FT_PONTO_EXTRA_AGREGADA): Deve conter os ids de ligação das tabelas de dimensões (Dimensão Calendário e Ponto de Venda) e a soma de ponto extras de linhas de produto agrupadas por ponto de venda no mês de Setembro/20.

Obs: Os dados de “Ponto Extra” estão categorizados na coluna TIPO_COLETA com o valor “Ponto Extra”

- [FT_PONTO_EXTRA](./Transformations/FT_PONTO_EXTRA.ktr)
- [FT_PONTO_EXTRA_AGREGADA](./Transformations/FT_PONTO_EXTRA_AGREGADA.ktr)

