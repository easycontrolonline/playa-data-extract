# Playa Data Extract

## Sobre

Sistema para conversão de imagens de documentos em dados estruturados usando IA.

## Funcionalidades

- Extração de dados de imagens com IA
- Gerenciamento de schemas JSON
- Processamento assíncrono em massa
- Armazenamento no AWS S3
- Exportação em XML e Excel

## Tecnologias

- Payload CMS + Next.js
- PostgreSQL
- AWS (S3, Lambda, SQS)

## Instalação

```bash
# Clone o repositório
git clone https://github.com/easycontrolonline/playa-data-extract.git

# Configure o ambiente
cp .env.example .env

# Instale dependências
pnpm install

# Inicie o servidor
pnpm dev
```

Acesse: http://localhost:3000/admin

## Requisitos

- Node.js v18+
- PostgreSQL 13+
- Conta AWS

---

Easycontrol © 2025
