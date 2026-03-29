# 🏨 Sistema de Agendamento em Produção

Sistema real de agendamento desenvolvido para um SPA em Serra Negra/SP, com foco em experiência do usuário e fluxo de conversão.

## 📋 Visão Geral

A aplicação permite que clientes realizem agendamentos em etapas guiadas, garantindo consistência de dados e validação em tempo real.

**Características Principais:**
- Fluxo de agendamento em múltiplas etapas
- Integração com backend via Supabase
- Validação forte de dados antes da persistência
- Sistema em produção com clientes reais

## 🏗️ Arquitetura

```
┌──────────────────┐
│  React Frontend  │ (Vite)
│  - Zustand Store │
└────────┬─────────┘
         │
         ├─────────────────────┐
         │                     │
         ▼                     ▼
    React Router         Zustand State
    (Navigation)         (Global State)
         │                     │
         └─────────────┬───────┘
                       │
                       ▼
        ┌──────────────────────────┐
        │  React Hook Form + Zod   │
        │  (Validation)            │
        └────────┬─────────────────┘
                 │
                 ▼
        ┌──────────────────────────┐
        │  Supabase API            │
        │  (Backend & DB)          │
        └──────────────────────────┘
```

## 🎯 Principais Decisões Técnicas

### 1. **Zustand para State Management**
Gerenciamento simples e eficaz do estado entre etapas:

```typescript
const useBookingStore = create((set) => ({
  step: 1,
  formData: {},
  advance: () => set((s) => ({ step: s.step + 1 })),
  updateData: (data) => set((s) => ({ 
    formData: { ...s.formData, ...data } 
  }))
}));
```

### 2. **React Hook Form + Zod**
Validação robusta antes de enviar ao backend:

```typescript
const schema = z.object({
  service: z.enum(['massage', 'facial']),
  date: z.date().min(new Date()),
  time: z.string(),
  name: z.string().min(3)
});

const form = useForm({
  resolver: zodResolver(schema)
});
```

### 3. **Supabase para Backend**
Backend serverless com segurança integrada:
- Autenticação com JWT
- Row-level security para isolamento de dados
- Realtime updates para confirmações
- Storage para imagens de serviços

### 4. **React Router para Navegação**
Estrutura clara das rotas:
- `/booking` → Iniciar agendamento
- `/booking/select-service` → Escolher serviço
- `/booking/select-datetime` → Data e hora
- `/booking/confirm` → Confirmação final

## 🔧 Stack Utilizado

| Aspecto | Tecnologia |
|---------|-----------|
| Frontend | React 18 |
| Linguagem | TypeScript |
| Build | Vite |
| State | Zustand |
| Forms | React Hook Form |
| Validação | Zod |
| Backend | Supabase |
| Styling | Tailwind CSS |

## 📊 Fluxo de Agendamento

```
1. Seleção de Serviço
   ↓
2. Escolha de Data e Hora
   ↓
3. Dados Pessoais
   ↓
4. Revisão e Confirmação
   ↓
5. Sucesso + Confirmação por Email
```

## 🚀 Como Rodar Localmente

```bash
# Clonar repositório
git clone https://github.com/usuario/spa-agendamento.git
cd spa-agendamento

# Instalar dependências
npm install

# Configurar variáveis de ambiente
cp .env.example .env.local
# Adicionar: VITE_SUPABASE_URL, VITE_SUPABASE_KEY

# Iniciar desenvolvimento
npm run dev

# Acesso: http://localhost:5173
```

## 🧪 Testes

```bash
# Testes unitários
npm run test

# Testes de componentes
npm run test:components

# Cobertura
npm run test:coverage
```

## 🎓 Principais Aprendizados

✅ **Construção de sistemas voltados para usuários reais**  
✅ **Importância de validação e consistência de dados**  
✅ **Design de fluxo orientado à experiência**  
✅ **Integração com backends serverless**  
✅ **State management escalável com Zustand**  
✅ **Performance em aplicações React**

## 📊 Estatísticas em Produção

- **Status:** Ativo
- **Clientes:** SPA em Serra Negra/SP
- **Uptime:** 99.9%
- **Agendamentos/mês:** 200+

## 🔒 Segurança

- ✅ Validação de entrada com Zod
- ✅ HTTPS obrigatório
- ✅ JWT em Supabase
- ✅ RLS (Row-Level Security) para isolamento

---

## 📞 Contato

GitHub: [seu-usuario](https://github.com)  
App: [agendamento.spa.com](https://agendamento.spa.com)

**Last Updated:** Março 2026
