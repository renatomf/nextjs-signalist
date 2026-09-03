<div align="center">

# 📈 Signalist

### Plataforma de rastreamento de ações em tempo real, alertas personalizados e insights de empresas

[![Next.js](https://img.shields.io/badge/Next.js-15.5.2-black?logo=next.js)](https://nextjs.org)
[![React](https://img.shields.io/badge/React-19.1-61DAFB?logo=react)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript)](https://www.typescriptlang.org)
[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?logo=mongodb)](https://www.mongodb.com)
[![Better Auth](https://img.shields.io/badge/Auth-Better%20Auth-6C47FF)](https://www.better-auth.com)
[![Inngest](https://img.shields.io/badge/Jobs-Inngest-000000)](https://www.inngest.com)
[![Deploy](https://img.shields.io/badge/demo-vercel-black?logo=vercel)](https://nextjs-stock-three.vercel.app)

</div>

---

## 📋 Sobre o projeto

**Signalist** é uma aplicação full-stack construída com **Next.js 15 (App Router)** que permite acompanhar preços de ações em tempo real, criar **alertas personalizados** e explorar **insights detalhados de empresas**. O projeto combina dados de mercado, autenticação de usuários, workflows orientados a eventos e notificações por e-mail em uma única plataforma.

## ✨ Funcionalidades

| Funcionalidade | Descrição |
|---|---|
| 📊 **Dashboard de ações** | Acompanhamento de preços em tempo real com gráficos interativos, filtráveis por setor, desempenho ou capitalização de mercado |
| 🔍 **Busca inteligente** | Localização rápida de ativos por ticker ou nome da empresa |
| ⭐ **Watchlist & Alertas** | Criação de listas de acompanhamento personalizadas e alertas de preço com notificação por e-mail |
| 🏢 **Insights de empresas** | Dados financeiros detalhados, ratings de analistas e indicadores fundamentais |
| ⚙️ **Workflows em tempo real** | Automação de atualizações de preço, geração de relatórios e resumos via Inngest |
| 📩 **Digests e notificações** | Resumos diários, alertas de earnings e notificações customizáveis por e-mail (Nodemailer) |
| 🔐 **Autenticação segura** | Login/sessão gerenciados via Better Auth, com rotas protegidas por middleware |

---

## 🛠️ Tech Stack

| Camada | Tecnologia |
|---|---|
| **Framework** | Next.js 15 (App Router, Turbopack) |
| **UI** | React 19, Tailwind CSS 4, Radix UI, `cmdk` (command palette), `sonner` (toasts) |
| **Formulários** | React Hook Form |
| **Autenticação** | Better Auth |
| **Banco de dados** | MongoDB + Mongoose (ODM) |
| **Jobs / Eventos** | Inngest (workflows assíncronos e orientados a eventos) |
| **E-mail** | Nodemailer |
| **Dados de mercado** | API externa de cotações (ex.: Finnhub) |
| **Linguagem** | TypeScript 5 |

---

## 🏗️ Arquitetura

```mermaid
flowchart TB
    User(["👤 Usuário"])

    subgraph Client["Frontend — Next.js App Router (React 19)"]
        UI["Dashboard / Watchlist / Busca<br/>(Tailwind + Radix UI + cmdk)"]
        Forms["Formulários<br/>(React Hook Form)"]
        UI --- Forms
    end

    MW[["Middleware<br/>(proteção de rotas)"]]
    Auth[["Better Auth<br/>Autenticação & sessão"]]
    API["API Routes / Server Actions<br/>(app/)"]
    DB[("MongoDB<br/>via Mongoose — database/")]
    Inngest[["Inngest<br/>Workflows em background"]]
    Market["API externa de mercado<br/>(cotações em tempo real)"]
    Mail[["Nodemailer<br/>Envio de e-mails"]]

    User --> UI
    UI -- requisições --> MW
    MW -- valida sessão --> Auth
    MW -- libera acesso --> API

    API -- lê/escreve dados --> DB
    API -- consulta preços/insights --> Market
    API -- dispara eventos --> Inngest

    Inngest -- busca cotações e gera digests --> Market
    Inngest -- persiste alertas/histórico --> DB
    Inngest -- envia alertas e resumos --> Mail
    Mail -- notifica --> User
```

### Como funciona o fluxo

1. **Usuário** acessa o dashboard, busca ativos e monta sua *watchlist* pela interface Next.js/React.
2. O **middleware** intercepta as requisições e valida a sessão junto ao **Better Auth**, protegendo rotas autenticadas (ex.: `/dashboard`, `/watchlist`).
3. As **API Routes / Server Actions** (dentro de `app/`) tratam a lógica de negócio, lendo e gravando dados no **MongoDB** através dos modelos Mongoose (`database/`).
4. Consultas de preços e dados de empresas são feitas a uma **API externa de mercado** (cotações em tempo real).
5. Eventos como "novo alerta criado" ou "preço atingiu o limite" são publicados no **Inngest**, que orquestra workflows assíncronos: monitoramento periódico de preços, geração de resumos/digests e verificação de condições de alerta.
6. Quando um workflow do Inngest identifica que uma notificação deve ser enviada, ele aciona o **Nodemailer** para disparar o e-mail (alerta de preço, digest diário, notificação de earnings) ao usuário.

> Ajuste as setas e nomes de serviços conforme a implementação exata do seu fork — em particular, confirme qual provedor de dados de mercado e qual serviço SMTP estão configurados no projeto.

---

## 📁 Estrutura do projeto

```
nextjs-stock/
├── app/                  # Rotas, páginas e layouts (Next.js App Router)
├── components/           # Componentes de UI reutilizáveis (shadcn/Radix)
├── database/             # Conexão com MongoDB e modelos Mongoose
├── hooks/                # React hooks customizados
├── lib/                  # Funções utilitárias, clients de API, configs (ex.: Better Auth, Inngest)
├── middleware/            # Middleware de proteção de rotas
├── public/assets/        # Imagens, ícones e assets estáticos
├── scripts/               # Scripts utilitários (ex.: test-db.mjs)
├── types/                 # Tipagens TypeScript compartilhadas
├── components.json        # Configuração do shadcn/ui
├── next.config.ts
├── eslint.config.mjs
├── postcss.config.mjs
├── tsconfig.json
├── package.json
└── README.md
```

---

## ⚙️ Pré-requisitos

- **Node.js** 20+
- Um gerenciador de pacotes: `npm`, `yarn`, `pnpm` ou `bun`
- Uma instância **MongoDB** (local ou Atlas)
- Chave de API de um provedor de dados de mercado (ex.: Finnhub)
- Credenciais SMTP para envio de e-mail (Nodemailer)
- Conta no **Inngest** (para orquestração de workflows)

---


## 📜 Scripts disponíveis

| Comando | Descrição |
|---|---|
| `npm run dev` | Inicia o servidor de desenvolvimento (Turbopack) |
| `npm run build` | Gera o build de produção (Turbopack) |
| `npm run start` | Inicia o servidor em modo produção |
| `npm run lint` | Roda o ESLint no projeto |
| `npm run test:db` | Testa a conexão com o MongoDB (`scripts/test-db.mjs`) |

---

## 🗺️ Roadmap

- [ ] Documentar os schemas do MongoDB (`database/`)
- [ ] Detalhar as functions do Inngest (`lib/inngest` ou equivalente)
- [ ] Mapear todas as rotas do App Router
- [ ] Adicionar testes automatizados
- [ ] Definir licença do projeto


<div align="center">

Feito por [@renatomf](https://github.com/renatomf)

</div>
