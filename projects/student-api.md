# Student Management API

API REST completa para gerenciamento de estudantes, cursos e matrículas, construída com foco em organização de código e boas práticas de backend moderno.

## 📋 Visão Geral

O projeto simula um sistema real com múltiplas entidades e regras de negócio.

**Objetivo:**
Demonstrar como construir APIs escaláveis com arquitetura em camadas, validação robusta e tratamento centralizado de erros.

## � Principais Decisões Técnicas

### 1. **Arquitetura em Camadas**
Separação clara de responsabilidades:

```
Controllers  ← Orquestração HTTP
    ↓
Services    ← Lógica de negócio
    ↓
Repositories ← Acesso a dados
    ↓
Database    ← Persistência
```

### 2. **Validação com Zod**
Validação robusta em todas as entradas:

```typescript
const createStudentSchema = z.object({
  name: z.string().min(3).max(255),
  email: z.string().email(),
  enrollmentDate: z.date()
});
```

### 3. **Prisma ORM**
Modelagem relacional segura e type-safe:
- Migrations automáticas
- Type safety em queries
- Suporte a relações complexas

### 4. **Rate Limiting**
Proteção contra abuso:

```typescript
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100
});
```

### 5. **Logging Estruturado com Winston**
Logs estruturados para melhor debugging e monitoramento

## 🔧 Stack Utilizado

| Aspecto | Tecnologia |
|---------|-----------|
| Framework | Express.js |
| Linguagem | TypeScript |
| Database | PostgreSQL + TypeORM |
| Validação | Zod / Joi |
| Testes | Jest + Supertest |
| Documentação | Swagger/OpenAPI |
| Containerização | Docker |

## 📊 Endpoints Principais

```
POST   /api/students              - Criar novo aluno
GET    /api/students               - Listar alunos (com paginação)
GET    /api/students/:id           - Buscar aluno específico
PUT    /api/students/:id           - Atualizar aluno
DELETE /api/students/:id           - Deletar aluno
GET    /api/students/:id/courses   - Cursos do aluno
```

## 🚀 Como Rodar

```bash
# Instalar dependências
npm install

# ORM | Prisma |
| Database | SQLite |
| Validação | Zod |
| Logging | Winston
npm run db:migrate

# Iniciar em desenvolvimento
npm run dev

# Acessar documentação
http://localhost:3000/api/docs
```

## 🧪 Testes

```bash
# Testes unitários
npm run test

# Testes de integração
npm run test:integration

# Cobertura de testes
npm run test:coverage

# Watch mode
npm run test:watch
```

## 📝 Exemplo de Uso

```bash
# Criar aluno
curl -X POST http://localhost:3000/api/students \
  -H "Content-Type: application/json" \
  -d '{
    "name": "João Silva",
    "email": "joao@example.com",
    "enrollmentDate": "2024-03-01"
  }'

# Listar com paginação
curl "http://localhost:3000/api/students?page=1&limit=10"
```

---

## 🎓 Principais Aprendizados

✅ **Organização de código para crescimento do sistema**  
✅ **Importância de validação e segurança**  
✅ **Modelagem de relações complexas com Prisma**  
✅ **Tratamento centralizado de erros**  
✅ **Rate limiting e proteção de endpoints**

---

## 📁 Estrutura de Pastas

```
src/
├── controllers/      # Handlers HTTP
├── services/         # Lógica de negócio
├── repositories/     # Acesso a dados
├── entities/         # Modelos de domínio
├── dto/              # Data Transfer Objects
├── middlewares/      # Express middlewares
├── config/           # Configurações
└── tests/            # Testes
```

---

## 📞 Contato

GitHub: [seu-usuario](https://github.com)  
LinkedIn: [seu-perfil](https://linkedin.com)

**Last Updated:** Março 2026
