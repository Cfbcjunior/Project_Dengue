# 🦟 Projeto Dengue: Pipeline de Dados com Arquitetura Medallion

Este repositório contém o desenvolvimento de um pipeline de Engenharia de Dados de ponta a ponta construído no **Databricks**, focado no processamento e análise de dados de notificações de Dengue no Brasil. 

O projeto aplica as melhores práticas da **Arquitetura Medallion** (Bronze, Silver e Gold) utilizando **PySpark** e **Delta Lake**, transformando arquivos brutos (CSVs) em um modelo de dados otimizado (Star Schema) pronto para consumo por ferramentas de Business Intelligence (BI) e tomada de decisão estratégica.

## 🎯 Objetivo do Projeto
Garantir a governança, qualidade e rastreabilidade dos dados de saúde pública, criando uma base analítica confiável para o monitoramento de casos de dengue. O pipeline foi desenhado para ser performático, escalável e autossuficiente.

## 🛠️ Stack Tecnológico
- **Computação Distribuída:** Apache Spark (PySpark)
- **Ambiente em Nuvem:** Databricks
- **Armazenamento:** Delta Lake / Unity Catalog
- **Linguagem:** Python

## 🏗️ Arquitetura do Pipeline

O fluxo de dados foi estruturado em três camadas progressivas de qualidade:

### 🥉 Camada Bronze (Raw Data)
- **Ingestão:** Leitura de múltiplos arquivos `.csv` de notificações diretamente de um Volume no Unity Catalog.
- **Rastreabilidade:** Adição de metadados (`_metadata.file_path`) para identificar a origem exata de cada registro.
- **Armazenamento:** Salvo no formato Delta, mantendo a integridade do dado bruto sem alterações.

### 🥈 Camada Silver (Cleansed & Enriched Data)
- **Deduplicação Robusta:** Utilização da flag nativa do SINAN (`NDUPLIC_N`) combinada com uma chave composta (Município, Data de Notificação, Ano de Nascimento, Sexo e Data do Primeiro Sintoma) para garantir a unicidade dos casos.
- **Data Cleansing:** Tratamento de valores nulos em colunas críticas, padronização de strings (Trim e Upper) e conversão de tipos de dados (Cast de Datas).
- **Redução de Dimensionalidade:** Exclusão de colunas de controle do sistema que não agregam valor analítico, otimizando o armazenamento e a performance.
- **Enriquecimento Nativo:** Tradução de códigos numéricos para descrições textuais (ex: Raça, Gestante) e mapeamento direto das Unidades Federativas (UFs) via funções `when` no PySpark, eliminando a necessidade de joins com planilhas externas e aumentando a resiliência do pipeline.

### 🥇 Camada Gold (Business-Ready Data)
- **Modelagem Dimensional:** Estruturação dos dados em um **Star Schema** para otimizar consultas analíticas.
- **Tabelas de Dimensão:** Criação de dimensões com chaves únicas (MD5 e formatação de data) para `Tempo`, `Município`, `Paciente` e `Gravidade`.
- **Tabela Fato:** Consolidação das métricas quantitativas (`fato_casos`), relacionando os IDs das dimensões e agregando a volumetria de notificações.

## 🚀 Como Executar
Os notebooks estão numerados em ordem de execução lógica:
1. `01_ingestao.ipynb` - Configurações iniciais e mapeamento de volumes.
2. `02_bronze.ipynb` - Ingestão dos dados brutos.
3. `03_silver.ipynb` - Limpeza, deduplicação e enriquecimento.
4. `04_gold.ipynb` - Modelagem dimensional (Star Schema).
5. `05_analytics.ipynb` - Consultas e validações analíticas.

## 👩‍💻 Autora
**Carlos Fernando Cardoso Junior**
[LinkedIn](http://linkedin.com/in/carlos-cardoso-junior-a7503bb4)
