# 🚀 Portfolio Dev

Bem-vindo! Aqui estão os projetos que demonstram minha experiência em desenvolvimento de software, com foco em decisões técnicas e cenários de produção.

---

## 📌 Projetos em Destaque

### 1. 🔥 **Hermes Observability Platform**
**Processamento de métricas em tempo real com arquitetura distribuída**

Plataforma de observabilidade desenvolvida para ingestão, processamento e análise de métricas em tempo real, simulando um cenário de alta concorrência e volume de dados.

O projeto foi estruturado com foco em escalabilidade e tolerância a falhas, separando responsabilidades entre ingestão, processamento e persistência.

**Principais decisões técnicas:**
- Redis Streams com Consumer Groups para garantir processamento confiável e evitar perda de dados
- Confirmação manual (ack) apenas após persistência completa, garantindo consistência
- Separação entre Collector e Processor para desacoplamento da ingestão
- PostgreSQL + TimescaleDB para armazenamento de séries temporais
- Pool de conexões configurado e monitorado para evitar gargalos

**Tecnologias utilizadas:**
Node.js · TypeScript · Redis · PostgreSQL · TimescaleDB · Docker · React

**Diferenciais:**
- Processamento assíncrono com controle de falhas
- Arquitetura inspirada em sistemas distribuídos reais
- Monitoramento de conexões e eventos críticos

**Principais aprendizados:**
- Controle de concorrência em sistemas distribuídos
- Garantia de consistência em pipelines assíncronos
- Impacto real de decisões de arquitetura em performance

📁 [GitHub](https://github.com) | 🚀 [Live](https://hermes-demo.com)

---

### 2. 🎯 **Sistema de Agendamento em Produção**
**SPA com fluxo completo e integração com Supabase**

Sistema real de agendamento desenvolvido para um SPA em Serra Negra/SP, com foco em experiência do usuário e fluxo de conversão.

A aplicação permite que clientes realizem agendamentos em etapas guiadas, garantindo consistência de dados e validação em tempo real.

**Principais decisões técnicas:**
- Gerenciamento de estado global com Zustand para manter o fluxo entre etapas
- Validação robusta com React Hook Form + Zod antes de enviar dados ao backend
- Integração com Supabase para persistência segura e comunicação via API
- Separação clara de rotas com React Router

**Tecnologias utilizadas:**
React · TypeScript · Vite · Zustand · React Hook Form · Zod · Supabase · Tailwind CSS

**Diferenciais:**
- Sistema em produção sendo utilizado por clientes reais
- Foco em UX e fluxo de dados completo
- Validação forte antes da persistência

**Principais aprendizados:**
- Construção de sistemas voltados para usuários reais
- Importância de validação e consistência de dados
- Design de fluxo orientado à experiência

📁 [GitHub](https://github.com) | 🚀 [Live](https://app-exemplo.com)

---

### 3. 📚 **Student Management API**
**API REST com arquitetura em camadas e validação robusta**

API REST completa para gerenciamento de estudantes, cursos e matrículas, construída com foco em organização de código e boas práticas de backend moderno.

O projeto simula um sistema real com múltiplas entidades e regras de negócio.

**Principais decisões técnicas:**
- Arquitetura em camadas (Controllers, Services, Repositories)
- Validação com Zod em todas as entradas
- Rate limiting para proteção da API
- Logging estruturado com Winston
- Uso do Prisma para modelagem relacional

**Tecnologias utilizadas:**
Node.js · TypeScript · Prisma · SQLite · Zod · Winston · Express

**Diferenciais:**
- Estrutura pronta para escalar
- Separação clara de responsabilidades
- Tratamento centralizado de erros

**Principais aprendizados:**
- Organização de código para crescimento do sistema
- Importância de validação e segurança
- Modelagem de relações complexas

📁 [GitHub](https://github.com) | 📖 [Documentação](https://api-docs.com)

---

### 4. 🔐 **Movie API**
**API com autenticação JWT e controle de acesso por usuário**

API REST desenvolvida para gerenciamento de filmes com autenticação e isolamento de dados por usuário.

O projeto foi pensado para simular um ambiente real com foco em segurança e organização.

**Principais decisões técnicas:**
- Autenticação com JWT
- Middleware de autorização para proteger rotas
- Pool de conexões com PostgreSQL
- Uso de asyncHandler para evitar repetição de try/catch
- Tratamento centralizado de erros

**Tecnologias utilizadas:**
Node.js · Express · PostgreSQL · JWT · Docker

**Diferenciais:**
- Controle de acesso por usuário
- Estrutura pronta para produção
- Organização em camadas

**Principais aprendizados:**
- Segurança em APIs REST
- Controle de concorrência no banco
- Organização de código escalável

📁 [GitHub](https://github.com)

---

### 5. 🤖 **Finance AI**
**Análise financeira com integração de IA e geração de insights**

Aplicação focada em análise de gastos com uso de inteligência artificial para geração de insights estruturados.

O projeto foi desenvolvido com foco em segurança, controle de uso e confiabilidade das respostas da IA.

**Principais decisões técnicas:**
- Uso de prefill para garantir respostas em JSON válido
- Rate limiting para controle de custos da API
- Sanitização de inputs contra prompt injection
- Geração de gráficos e exportação em PDF

**Tecnologias utilizadas:**
React · Vercel · Serverless Functions · API de IA

**Diferenciais:**
- Integração real com IA
- Preocupação com segurança e custo
- Geração de relatórios automatizados

**Principais aprendizados:**
- Controle de comportamento de modelos de IA
- Segurança em aplicações com IA
- Integração de front + backend + IA

📁 [GitHub](https://github.com)

---

## 🛠️ Stack Principal

**Backend:** Node.js · TypeScript · Express · Fastify
**Frontend:** React · TypeScript · Redux · Next.js
**Banco de Dados:** PostgreSQL · Redis · MongoDB
**DevOps:** Docker · Docker Compose · Git
**Testes:** Jest · Vitest · Testing Library
**Ferramentas:** ESLint · Prettier · Husky

---

## 📊 Estatísticas

- **5+ projetos em produção**
- **1000+ horas de desenvolvimento**
- **Foco em performance e escalabilidade**
- **Código mantido com padrões de qualidade altos**

---

## 🎓 Sobre Mim

Desenvolvedor fullstack com foco em construir sistemas escaláveis e confiáveis. Experiência em arquitetura de software, otimização de performance e boas práticas de engenharia.

---

## 📋 Como Usar Este Repositório

Cada projeto tem sua própria pasta com:
- `README.md` - Documentação completa
- `src/` - Código-fonte
- `tests/` - Testes automatizados
- `docs/` - Documentação técnica

---

## 🔗 Links Importantes

- 💼 [LinkedIn](https://linkedin.com/in/francisco-jefferson-batista-honorato)
- 🐙 [GitHub](https://github.com/FranciscoHonorat)
- 📧 Email: jeffhonorato230@gmail.com

---

**Atualizado em:** Março 2026
