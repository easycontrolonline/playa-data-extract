
## 1. Introdução

O objetivo deste projeto é desenvolver um sistema robusto para converter imagens de documentos escaneados em dados estruturados. Essa conversão será realizada por meio de modelos de IA capazes de extrair informações com base em _schemas_ JSON predefinidos ("Output Structured Schemas"). O sistema suportará processamento em massa de forma assíncrona, armazenamento dos arquivos no AWS S3 e exportação dos dados extraídos em formatos XML e Excel.

## 2. Objetivos e Escopo

### 2.1. Objetivos

- **Extração de Dados:** Converter imagens de documentos (.jpg) em dados estruturados, utilizando modelos de IA.

- **Gerenciamento de Schemas:** Permitir o cadastro e a configuração de múltiplos _schemas_, cada um com parâmetros específicos (incluindo o próprio JSON e prompts adicionais).

- **Processamento Assíncrono:** Suporte à conversão em massa com envio de imagens para processamento via fila.

- **Armazenamento e Exportação:** Armazenar resultados estruturados em arquivos XML, Excel e/ou em banco de dados (SQL com JSONB ou NoSQL) e possibilitar exportação XML e Excel.

- **Integração com AWS:** Utilização do AWS S3 para armazenar os arquivos de imagem, arquivos xml/excel e relatórios gerados para exportação.

- **Logs e Monitoramento:** Registros detalhados (logs) e monitoramento contínuo dos processos.



### 2.2. Escopo

- **Sprint 1 (MVP):** 

    - Front-end web com autenticação e registro de usuários.
    - Banco de dados para armazenamento dos _schemas_ e dos dados extraídos.
    - Cadastro de _schemas_.
    - Upload assíncrono de arquivos de imagens para o AWS S3.
    - Testes e refinamento do _schema_ para conversão de documentos em dados estruturados (Faturas).
    - Processamento assíncrono via fila para envio dos arquivos para conversão pelo modelo de IA.
    - Armazenamento dos dados extraídos no banco de dados.
    - Exportação dos dados em XML e Excel. (exportação por fatura)
    - Relatório básico de processamento (logs).

    

---

## 3. Requisitos do Sistema

### 3.1. Requisitos Funcionais

- **Cadastro e Gerenciamento de Schemas:**
  
    - Permitir criação, edição e remoção de _schemas_ de conversão.
    - Configurar parâmetros específicos, como o JSON do _schema_ e prompts adicionais.
- **Configuração Global de Conversão:**
  
    - Definir parâmetros globais, como o modelo de IA a ser utilizado e configurações de prompt que se aplicam a todas as conversões.
- **Armazenamento de Dados:**
  
    - Salvar _schemas_ e dados extraídos (JSON) em um banco de dados, utilizando soluções como:
        - Postgres com JSONB (para emulação de funcionalidades NoSQL).
        - Alternativamente, MongoDB, DynamoDB ou Firebase.
- **Processamento de Arquivos:**
  
    - Enviar imagens para processamento:
        - Upload para AWS S3.
        - Envio junto com o _schema_ e parâmetros para o processamento assíncrono.
    - Implementar um sistema de fila (ex.: AWS SQS ou RabbitMQ) para orquestrar o processamento dos arquivos.
- **Exportação de Dados:**
  
    - Permitir a exportação dos dados estruturados extraídos para formatos XML e Excel.

### 3.2. Requisitos Não Funcionais

- **Escalabilidade:**
  
    - Suporte a conversões em massa com processamento assíncrono e escalabilidade horizontal.
- **Desempenho:**
  
    - Processamento eficiente e rápido, com monitoramento e logs para identificar falhas e gargalos de processamento.
- **Segurança:**
  
    - Implementar autenticação e autorização robustas.
    - Proteger o acesso aos dados e aos arquivos armazenados (ex.: criptografia, políticas de acesso no AWS S3).
- **Confiabilidade e Monitoramento:**
  
    - Registros detalhados (logs) e monitoramento contínuo dos processos.
    - Tratamento adequado de falhas e retries em caso de erros de processamento.
- **Usabilidade:**
  
    - Interface de usuário intuitiva para cadastro de _schemas_, upload de arquivos e visualização dos resultados.

---

## 4. Arquitetura do Sistema

### 4.1. Visão Geral

O sistema será estruturado em camadas, possibilitando a separação de responsabilidades e facilitando a escalabilidade e manutenção. A seguir, uma visão geral dos componentes principais:

- **Front-end:** Interface web para interação dos usuários (cadastro de _schemas_, upload de arquivos, visualização de logs e exportação de dados).
- **Back-end:** APIs e serviços responsáveis pelo processamento, armazenamento e orquestração das tarefas.
- **Armazenamento de Arquivos:** AWS S3 para o upload e armazenamento de imagens.
- **Banco de Dados:** Uso de Postgres (com JSONB) ou alternativas NoSQL para armazenamento dos _schemas_ e dos dados extraídos.
- **Sistema de Fila:** Serviço de mensageria (AWS SQS, RabbitMQ) para gerenciamento do processamento assíncrono.

### 4.2. Tecnologias Sugeridas

#### Back-end

- **Linguagem/Framework:**
    - _Node.js_ (por sua flexibilidade e ecossistema robusto) ou _Micronaut_ (para aplicações Java de alta performance).
- **Processamento Assíncrono:**
    - Utilização de AWS Lambda para processamento escalável ou containers gerenciados com Kubernetes.
- **Fila de Mensagens:**
    - AWS SQS para integração nativa com outros serviços AWS, ou RabbitMQ para maior controle sobre a fila.

#### Banco de Dados

- **SQL:**
    - PostgreSQL com JSONB, que permite armazenar documentos JSON com alta performance e  flexibilidade.
- **NoSQL:**
    - MongoDB Atlas ou DynamoDB, conforme necessidades de escalabilidade e modelo de dados.

#### Armazenamento

- **AWS S3:**
    - Para armazenamento seguro e escalável dos arquivos de imagem.

#### Front-end

- **Framework:**
    - React ou NextJS, para uma aplicação web moderna e responsiva.
- **Funcionalidades:**
    - Upload de arquivos, seleção de _schemas_, visualização de logs, interface para testes de conversão e exportação de resultados.

---

## 5. Fluxo de Processamento

### 5.1. Fluxo Principal

1. **Cadastro do Schema:**
   
    - Criação de um novo _schema_ (ex.: `Invoice Schema`) com os parâmetros específicos e JSON de configuração.
2. **Início do Processo de Conversão:**
   - O usuário inicia uma nova conversão ou extração de dados.
    - Seleciona o _schema_ previamente cadastrado.
    - Seleciona um ou mais arquivos de imagem para conversão.
3. **Upload e Armazenamento:**
   
    - Os arquivos são enviados para o AWS S3.
    - Metadados (referências aos arquivos, _schema_ utilizado e parâmetros) são registrados no banco de dados.
4. **Enfileiramento e Processamento:**
   
    - Os arquivos são adicionados a uma fila de processamento.
    - Um ou mais workers consomem a fila, processando os arquivos sequencialmente ou em paralelo, enviando-os para o modelo de IA configurado.
    - Os dados extraídos (JSON) são recebidos e armazenados no banco de dados.
5. **Finalização e Exportação:**
   
    - Após o término do processamento, o sistema exibe uma mensagem de finalização com um resumo dos logs.
    - O usuário tem a opção de exportar os dados extraídos em formato XML e Excel.

### 5.2. Considerações de Processamento

- **Paralelismo e Escalabilidade:**
    - O sistema deve permitir a expansão horizontal dos workers para processar grandes volumes de arquivos.
- **Monitoramento e Logs:**
    - Registro detalhado de cada etapa do processamento, com logs de sucesso, falhas e métricas de desempenho.
- **Tratamento de Erros:**
    - Implementação de mecanismos de retry e fallback para casos de falhas temporárias na comunicação com a IA ou no upload para o S3.
    
    


## 6 Custo Operacional Estimado

### Premissas de Uso
Estimativa baseada no processamento de **15.000 imagens/mês** (~500 arquivos/dia) com tamanho médio de **1 MB por arquivo**.

- **Volume de dados:**
  - 15.000 arquivos × 1 MB = **15 GB/mês**
  - 500 arquivos/dia (~500 MB/dia)

- **Padrão de uso:**
  - Upload distribuído: ~40 arquivos/hora (considerando 12h de atividade)
  - Tempo médio de processamento por imagem: ~10-20 segundos (API da OpenAI)

### Detalhamento de Custos Mensais

#### **(1) AWS S3**
| Item | Cálculo | Custo Mensal |
|------|---------|------------|
| Armazenamento (15 GB) | $0.023/GB × 15 GB | $0.35 |
| Solicitações PUT (15.000) | $0.005 por 1.000 × 15 | $0.075 |
| Transferência para Lambda (15 GB) | $0.006/GB × 15 GB | $0.09 |
| **Subtotal AWS S3** | | **$0.52** |

#### **(2) AWS Lambda**
| Item | Cálculo | Custo Mensal |
|------|---------|------------|
| 15.000 execuções (10-20s cada) | $0.0000833335/execução × 15.000 | $1.25 |
| Memória adicional (256 MB vs 128 MB) | Fator de 2× | $1.25 |
| **Subtotal AWS Lambda** | | **$2.50** |

#### **(3) OpenAI API**
| Item | Cálculo | Custo Mensal |
|------|---------|------------|
| GPT-4 Vision (15.000 imagens) | $0.002/imagem × 15.000 | $30.00 |
| **Subtotal OpenAI API** | | **$30.00** |

#### **(4) Banco de Dados NoSQL**
| Item | Cálculo | Custo Mensal |
|------|---------|------------|
| MongoDB Atlas 10GB (2 GB x 2 vCPUs) | $0.08/hora × 240 horas | $19.20 |
| Uso otimizado (8h/dia) | $19.20 × (8/24) | $6.40 |
| **Subtotal Banco de Dados** | | **$6.40** |

#### **(5) VPS para Frontend**
| Item | Opções | Custo Mensal |
|------|--------|------------|
| VPS Básico (Hostinger) | 1-2 vCPUs, 2-4 GB RAM | $5.00 |
| VPS Intermediário | 2-4 vCPUs, 4-8 GB RAM | $10.00-$20.00 |
| **Subtotal VPS** | | **$5.00-$20.00** |

#### **(6) AWS SQS**
| Item | Cálculo | Custo Mensal |
|------|---------|------------|
| Processamento de mensagens (15.000) | 15.000 mensagens (dentro do Free Tier de 1 milhão) | $0.00 |
| **Subtotal AWS SQS** | | **$0.00** |

### Custo Total Estimado

| Componente | Custo Mensal |
|------------|------------|
| AWS S3 | $0.52 |
| AWS Lambda | $2.50 |
| OpenAI API | $30.00 |
| Banco de Dados | $6.40 |
| VPS Frontend | $5.00-$20.00 |
| AWS SQS | $0.00 |
| **TOTAL** | **$44.42-$59.42** |

> **Observação:** Os custos podem variar dependendo do modelo específico da OpenAI utilizado, otimizações de código, e padrões reais de uso. Recomenda-se monitorar o consumo nos primeiros meses para ajustes.



## 7. Cronograma de Desenvolvimento - Sprint 1

#### Cálculo do Prazo Total

| Categoria                        | Tarefas                                                      | Dias Estimados      |
| -------------------------------- | ------------------------------------------------------------ | ------------------- |
| Etapas Iniciais **(Realizadas)** | Reunião de alinhamento + Demonstração personalizada          | 2 dias              |
| Setup Inicial e Infraestrutura   | Configuração de repositório, AWS e ambiente                  | 2,5 dias            |
| Backend                          | Implementação de Lambda, processamento, armazenamento e relatórios | 10,5 dias           |
| Frontend                         | Implementação de interfaces e funcionalidades                | 18 dias             |
| Integração e Testes              | Testes de API, integração e performance                      | 6 dias              |
| Deploy                           | Configuração de ambiente e CI/CD                             | 1,5 dias            |
| Acompanhamento e Suporte         | Treinamento                                                  | 1 dia               |
| **Total**                        |                                                              | **41,5 dias úteis** |


---

## 8. Considerações Finais

Este projeto propõe uma solução moderna e escalável para a conversão de documentos escaneados em dados estruturados utilizando IA. A modularidade na arquitetura, a utilização de tecnologias como AWS S3, AWS SQS e bancos de dados que suportam JSON, aliadas a um front-end responsivo, garantem um sistema robusto, flexível e preparado para futuras expansões. A implementação das melhorias sugeridas (como geração automática de _schema_ e interface de teste) contribuirá para uma melhor experiência do usuário e maior integração com sistemas externos.

