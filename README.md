# Playa Data Extract

<p align="center">
  <img src="https://res.cloudinary.com/hczpmiapo/image/upload/v1732576652/Static%20assets/Logos/payload_V3_mhv6wc.png" alt="Payload CMS Logo" width="50" />
  <img src="https://cdn.worldvectorlogo.com/logos/aws-2.svg" alt="AWS Logo" width="50" style="margin-left: 20px" />
</p>

## Sobre o Projeto

O Playa Data Extract é um sistema robusto para converter imagens de documentos escaneados em dados estruturados utilizando modelos de Inteligência Artificial. O sistema suporta processamento em massa de forma assíncrona, armazenamento dos arquivos no AWS S3 e exportação dos dados extraídos em formatos XML e Excel.

## Funcionalidades Principais

- **Extração de Dados:** Conversão de imagens (.jpg) em dados estruturados utilizando modelos de IA
- **Gerenciamento de Schemas:** Cadastro e configuração de múltiplos schemas JSON para diferentes tipos de documentos
- **Processamento Assíncrono:** Conversão em massa com filas de processamento
- **Armazenamento na Nuvem:** Integração com AWS S3 para armazenamento de arquivos
- **Exportação Flexível:** Exportação dos dados em formatos XML e Excel
- **Monitoramento:** Logs detalhados e monitoramento contínuo dos processos

## Tecnologias Utilizadas

- **Frontend:** PayloadCMS (Admin UI) + React/Next.js
- **Backend:** Node.js + Payload CMS
- **Banco de Dados:** PostgreSQL com suporte a JSONB
- **Cloud Services:** AWS S3, AWS Lambda, AWS SQS
- **Processamento:** APIs de IA para extração de dados de documentos

## Arquitetura do Sistema

O sistema é estruturado em camadas para facilitar a manutenção e escalabilidade:

1. **Frontend:** Interface web para interação dos usuários
2. **Backend:** APIs e serviços para processamento e orquestração
3. **Armazenamento:** AWS S3 para arquivos e PostgreSQL para dados estruturados
4. **Processamento Assíncrono:** Sistema de filas para gerenciar conversões em massa

## Fluxo de Processamento

1. **Cadastro do Schema:** Criação de schemas JSON definindo a estrutura dos dados a serem extraídos
2. **Upload de Documentos:** Seleção do schema e upload de imagens para processamento
3. **Processamento na Nuvem:** Envio para serviços de IA via AWS Lambda
4. **Armazenamento de Resultados:** Salvamento dos dados extraídos no banco de dados
5. **Exportação:** Geração de arquivos XML e Excel com os dados estruturados

## Configuração Local

1. Clone o repositório: `git clone https://github.com/easycontrolonline/playa-data-extract.git`
2. Configure o arquivo `.env` conforme o exemplo em `.env.example`
3. Instale as dependências: `pnpm install` ou `npm install`
4. Inicie o servidor de desenvolvimento: `pnpm dev` ou `npm run dev`
5. Acesse o admin em: `http://localhost:3000/admin`

## Requisitos

- Node.js v18+ 
- PostgreSQL 13+
- Conta AWS (para S3, Lambda e SQS)
- API Key para o serviço de IA utilizado

## Escopo do Desenvolvimento

### Sprint 1 (MVP)
- Frontend web com autenticação e registro
- Cadastro de schemas
- Upload assíncrono para o AWS S3
- Processamento via fila
- Exportação em XML e Excel
- Relatórios básicos de processamento

## Licença

Este projeto está licenciado sob os termos da licença MIT.

---

<p align="center">Desenvolvido por Easycontrol</p>

