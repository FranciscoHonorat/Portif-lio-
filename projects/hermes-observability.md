# Hermes Observability Platform

Plataforma de observabilidade desenvolvida para ingestão, processamento e análise de métricas em tempo real, simulando um cenário de alta concorrência e volume de dados.

## 📋 Visão Geral

**Arquitetura e Foco:**
O projeto foi estruturado com foco em escalabilidade e tolerância a falhas, separando responsabilidades entre ingestão, processamento e persistência.

**Objetivo:**
Demonstrar como sistemas distribuídos reais lidam com volume alto de dados mantendo consistência e confiabilidade.

## 🏗️ Arquitetura

```
┌─────────────┐
│  Producers  │ (Services enviando métricas)
└──────┬──────┘
       │
       ▼
┌─────────────────┐
│ Redis Streams   │ (Buffer distribuído)
└──────┬──────────┘
       │
       ▼
┌─────────────────┐
│ Worker Pool     │ (processamento paralelo)
└──────┬──────────┘
       │
       ├──────────────┐
       ▼              ▼
   PostgreSQL     Redis Cache
```

## 🎯 Principais Decisões Técnicas

### 1. **Redis Streams com Consumer Groups**
- Garantia de processamento confiável e sem perda de dados
- Consumer Groups para distribuir processamento entre múltiplas instâncias
- Acknowledging manual (ack) apenas após persistência completa

```typescript
// Producer enviando métricas
await redis.xadd('metrics', '*', 'value', metric, 'timestamp', Date.now());

// Consumer lendo com garantia
const messages = await redis.xreadgroup(
  'GROUP', 'processors', 'worker-1',
  'STREAMS', 'metrics', '>'
);
```

### 2. **Separação Collector vs Processor**
- **Collector:** Recebe métricas rapidamente (ingest rápido)
- **Processor:** Valida, processa e persiste (trabalho pesado)
- Desacoplamento garante que métricas não sejam perdidas

### 3. **PostgreSQL + TimescaleDB**
- TimescaleDB para armazenamento otimizado de séries temporais
- Compressão automática de dados históricos
- Queries rápidas em grandes volumes

### 4. **Pool de Conexões Monitorado**
- Configuração estratégica para evitar gargalos
- Monitoramento de disponibilidade de conexões
- Alertas quando pool está saturado

## 🔧 Stack Utilizado
|
| Linguagem | TypeScript |
| Message Queue | Redis Streams |
| TSDB | PostgreSQL + TimescaleDB |
| Frontend | React |
| Containerização | Docker
| Frontend | React 18 + TypeScript |
| Containerização | Docker |
| Orquestração | Docker Compose |

## 📊 Performance

- **ThCaracterísticas

- **Ingestão:** Coleta rápida e confiável de métricas
- **Processamento:** Transformação e enriquecimento de dados
- **Consistência:** Garantia de entrega com consumer groups
- **Escalabilidade:** Múltiplos processadores trabalhando em paralelo

```bash
# Clonar repositório
git clone https://github.com/usuario/hermes-observability.git
cd hermes-observability

# Instalar dependências
npm install

# Usar Docker Compose
docker-compose up -d

# Rodar migrations
npm run db:migrate

# Iniciar workers
npm run workers:start

# Iniciar API
npm run dev
```

## 📝 Endpoints Principais

```
POST   /api/metrics/ingest      - Ingerir novas métricas
GET    /api/metrics/:id         - Buscar métrica específica
GET    /api/dashboards          - Listar dashboards
GET    /ws                      - WebSocket para updates real-time
```

## 🧪 Testes

```bash
# Testes unitários
npm run test

# Testes de integração
npm run test:integration

# Cobertura de testes
npm run test:coverage
```

---

## 🎓 Principais Aprendizados

✅ **Concorrência em Node.js:** Como gerenciar milhares de conexões simultâneas  
✅ **Message Queues:** Importância do desacoplamento em sistemas distribuídos  
✅ **Pertrole de concorrência em sistemas distribuídos**  
✅ **Garantia de consistência em pipelines assíncronos**  
✅ **Impacto real de decisões de arquitetura em performance**  
✅ **Design patterns para alta disponibilidade**  
✅ **Monitoramento e debug de sistemas distribuídos**

## 📞 Contato

- GitHub: [seu-usuario](https://github.com)
- LinkedIn: [seu-perfil](https://linkedin.com)

**Last Updated:** Março 2026
