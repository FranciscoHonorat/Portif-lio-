# Finance AI

Aplicação focada em análise de gastos com uso de inteligência artificial para geração de insights estruturados.

## 📋 Visão Geral

O projeto foi desenvolvido com foco em segurança, controle de uso e confiabilidade das respostas da IA.

**Características Principais:**
- Análise de gastos com IA
- Geração de insights estruturados
- Rate limiting para controle de custos
- Proteção contra prompt injection

## � Principais Decisões Técnicas

### 1. **Prefill para Respostas em JSON Válido**
Garantir que a IA sempre retorna JSON estruturado:

```typescript
const prompt = `Analise os gastos fornecidos.
Retorne uma resposta JSON com a estrutura:
{
  "summary": "...",
  "recommendations": ["..."],
  "priority": "high|medium|low"
}`;

// Usar prefill no completion
const response = await ai.complete({
  prompt,
  prefill: '{"summary": "'
});
```

### 2. **Rate Limiting para Controle de Custos**
Proteger contra abuso e custos altos com IA:

```typescript
const aiLimiter = rateLimit({
  windowMs: 60 * 60 * 1000, // 1 hora
  max: 50, // máximo 50 análises por hora
  message: 'Limite de análises excedido. Tente novamente mais tarde.'
});

router.post('/analyze', aiLimiter, analyzeHandler);
```

### 3. **Sanitização contra Prompt Injection**
Evitar que usuários manipulem o comportamento da IA:

```typescript
const sanitizeUserInput = (input) => {
  // Remover comandos potencialmente perigosos
  const dangerous = ['ignore previous', 'system:', 'admin:'];
  let sanitized = input;
  dangerous.forEach(pattern => {
    sanitized = sanitized.replace(new RegExp(pattern, 'gi'), '');
  });
  return sanitized;
};
```

### 4. **Geração de Gráficos e PDF**
Exportar análises em formatos visuais:
- Gráficos com Chart.js
- Exportação PDF com pdfkit
- Visualização de tendências

### 5. **Caching de Análises**
Evitar re-análise dos mesmos dados:

```typescript
const analysisCache = new Map();
const cacheKey = hash(userExpenses);

if (analysisCache.has(cacheKey)) {
  return analysisCache.get(cacheKey);
}

const analysis = await analyzeWithAI(expenses);
analysisCache.set(cacheKey, analysis);
```

## 🔧 Stack Utilizado

| Aspecto | Tecnologia |
|---------|-----------|
| Frontend | React |
| Deploy | Vercel |
| Backend | Serverless Functions |
| IA | API de IA (ChatGPT/Claude) |
| Gráficos | Chart.js |
| PDF | pdfkit |

## 📊 Funcionalidades Principais

- ✅ Análise de gastos com IA
- ✅ Geração de insights estruturados
- ✅ Gráficos de gastos por categoria
- ✅ Recomendações personalizadas
- ✅ Exportação em PDF
- ✅ Histórico de análises
- ✅ Rate limiting e proteção

## 🚀 Como Rodar Localmente

```bash
# Clonar repositório
git clone https://github.com/usuario/finance-ai.git
cd finance-ai

# Instalar dependências
npm install

# Configurar variáveis de ambiente
cp .env.example .env
# Adicionar: VITE_AI_API_KEY (chave da API de IA)

# Iniciar desenvolvimento
npm run dev

# Acesso: http://localhost:5173
```

## 🧪 Testes

```bash
# Testes de análise
npm run test:analysis

# Testes de segurança (prompt injection)
npm run test:security

# Cobertura
npm run test:coverage
```

## 📝 Exemplo de Uso

```typescript
// Frontend
const expenses = [
  { category: 'food', amount: 250 },
  { category: 'transport', amount: 150 },
  { category: 'entertainment', amount: 100 }
];

const response = await fetch('/api/analyze', {
  method: 'POST',
  body: JSON.stringify({ expenses })
});

const analysis = await response.json();
// {
//   summary: "Gastos com alimentação estão altos...",
//   recommendations: ["Reduzir gastos com delivery..."],
//   priority: "high"
// }
```

## 🎓 Principais Aprendizados

✅ **Controle de comportamento de modelos de IA**  
✅ **Segurança em aplicações com IA**  
✅ **Integração de front + backend + IA**  
✅ **Rate limiting para controle de custos**  
✅ **Sanitização de inputs para segurança**

---

## 📞 Contato

GitHub: [seu-usuario](https://github.com)

**Last Updated:** Março 2026
