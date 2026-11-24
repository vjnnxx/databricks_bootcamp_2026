# 🧱 Data Lakehouse no Databricks — Bootcamp 2026

Este repositório contém a implementação completa de um **Data Lakehouse** desenvolvido na plataforma **Databricks**, aplicando as melhores práticas de Engenharia e Análise de Dados modernas. O projeto estrutura um pipeline de dados de ponta a ponta utilizando a **Arquitetura Medallion (Bronze, Silver e Gold)** para transformar dados brutos de sistemas transacionais (CRM e ERP) em modelos dimensionais prontos para Business Intelligence (BI) e Analytics.

---

## 🛠️ Tecnologias Utilizadas

* **Plataforma:** Databricks
* **Processamento Distribuído:** Apache Spark (PySpark & Spark SQL)
* **Armazenamento & Transações ACID:** Delta Lake
* **Governança & Catálogo de Dados:** Unity Catalog (Schemas & Volumes)

---

## 🏗️ Arquitetura e Lógica de Funcionamento do Lakehouse

O pipeline foi desenhado seguindo o padrão **Medallion Architecture**, garantindo qualidade progressiva, rastreabilidade e governança dos dados à medida que avançam pelas camadas:

```text
[ Fontes de Dados (CSV: CRM & ERP) ]
                 │
                 ▼  (Ingestão via Unity Catalog Volumes)
┌──────────────────────────────────┐
│         CAMADA BRONZE            │  ──► Dados brutos (Raw) armazenados em formato Delta
└──────────────────────────────────┘      sem alterações estruturais (histórico fiel).
                 │
                 ▼  (Limpeza, Tipagem e Padronização)
┌──────────────────────────────────┐
│         CAMADA SILVER            │  ──► Dados higienizados, deduplicados, com tipos
└──────────────────────────────────┘      corrigidos e regras de negócio aplicadas.
                 │
                 ▼  (Modelagem Dimensional - Star Schema)
┌──────────────────────────────────┐
│          CAMADA GOLD             │  ──► Tabelas Fato e Dimensões otimizadas para
└──────────────────────────────────┘      consultas analíticas, dashboards e BI.
```

### Detalhamento das Camadas

1. **Inicialização (`init_lakehouse`):**
   * Configura a estrutura base no **Unity Catalog**, criando os schemas isolados (`bronze`, `silver` e `gold`) e o **Volume** gerenciado onde os arquivos brutos são depositados inicialmente.
2. **Camada Bronze (Ingestão Bruta):**
   * Realiza a leitura dos arquivos de origem (sistemas CRM e ERP) diretamente do Volume do Databricks e os persiste como tabelas **Delta Lake**. O objetivo desta camada é manter uma cópia fiel e auditável dos dados originais.
3. **Camada Silver (Refinamento e Qualidade):**
   * Aplica transformações via **PySpark** e **Spark SQL** para tratar valores nulos, remover duplicidades, padronizar formatos de texto/datas, validar chaves e realizar o *casting* adequado dos tipos de dados.
4. **Camada Gold (Camada de Negócio / Modelagem Dimensional):**
   * Consolida e integra as entidades da camada Silver em um modelo dimensional (**Star Schema**), estruturado em tabelas de **Dimensão** (ex.: clientes, produtos) e tabelas **Fato** (ex.: vendas), entregando alta performance para consumo analítico.

---

## 📂 Estrutura do Repositório

```text
databricks_bootcamp_2026/
├── datasets/               # Conjuntos de dados de origem (arquivos brutos de CRM, ERP, Vendas e RH)
├── docs/                   # Documentações auxiliares, diagramas e anotações do projeto
├── scripts/                # Notebooks e scripts de execução do pipeline
│   ├── init_lakehouse      # Script inicial para criação de schemas e volumes no Unity Catalog
│   ├── bronze/             # Notebooks de ingestão dos dados brutos para a camada Bronze
│   ├── silver/             # Notebooks de limpeza, tratamento e padronização (camada Silver)
│   └── gold/               # Notebooks de modelagem dimensional e regras de negócio (camada Gold)
└── README.md               # Documentação principal do projeto
```

---

## 🚀 Como Executar o Projeto

1. **Ambiente Databricks:** Certifique-se de ter acesso a um workspace do Databricks com **Unity Catalog** habilitado (como o *Databricks Free Edition* ou ambiente corporativo/cloud).
2. **Importação:** Clone ou importe este repositório diretamente no seu workspace utilizando o recurso **Databricks Git Folders (Repos)**.
3. **Upload dos Datasets:** Faça o upload dos arquivos da pasta `datasets/` para o Volume criado no Unity Catalog.
4. **Ordem de Execução dos Notebooks:**
   * **Passo 1:** Execute o notebook de inicialização (`init_lakehouse`) para criar os schemas e volumes necessários.
   * **Passo 2:** Execute os notebooks dentro de `scripts/bronze/` para carregar os dados brutos.
   * **Passo 3:** Execute os notebooks dentro de `scripts/silver/` para processar e limpar as tabelas.
   * **Passo 4:** Execute os notebooks dentro de `scripts/gold/` para construir as tabelas Fato e Dimensões.

---

## 🙏 Créditos e Referência ao Projeto Original

Este projeto foi desenvolvido durante a realização do **Databricks Bootcamp 2026**, ministrado por **Baraa Khatib Salkini (Data With Baraa)**.

* **Repositório Original:** [DataWithBaraa/databricks_bootcamp_2026](https://github.com/DataWithBaraa/databricks_bootcamp_2026/tree/main)
* **Autor / Instrutor:** [Data With Baraa](https://www.datawithbaraa.com)
