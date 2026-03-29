# Movie API

API REST desenvolvida para gerenciamento de filmes com autenticação e isolamento de dados por usuário.

## 📋 Visão Geral

O projeto foi pensado para simular um ambiente real com foco em segurança e organização.

**Características:**
- Autenticação com JWT
- Isolamento de dados por usuário
- Estrutura pronta para produção
- Organização em camadas

## 🏗️ Arquitetura

```
┌──────────────────────────────────┐
│   Client (REST Client)           │
└────────────┬─────────────────────┘
             │
             ▼
┌──────────────────────────────────┐
│   Express Server                 │
├──────────────────────────────────┤
│   Authentication Middleware      │ ← JWT verification
│   Authorization Middleware       │ ← User isolation
└────────────┬─────────────────────┘
             │
             ▼
┌──────────────────────────────────┐
│   Controllers & Handlers         │
│   (asyncHandler wrapper)         │
└────────────┬─────────────────────┘
             │
             ▼
┌──────────────────────────────────┐
│   PostgreSQL with Connection Pool│
└──────────────────────────────────┘
```

## 🎯 Principais Decisões Técnicas

### 1. **Autenticação com JWT**

```typescript
// Gerar token na autenticação
const token = jwt.sign(
  { userId: user.id, email: user.email },
  process.env.JWT_SECRET,
  { expiresIn: '7d' }
);

// Middleware para validar
const verifyToken = (req, res, next) => {
  const token = req.headers['authorization']?.split(' ')[1];
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
};
```

### 2. **Middleware de Autorização**
Garantir que usuários só acessem seus próprios dados:

```typescript
const ensureUserOwnership = (req, res, next) => {
  const movieUserId = req.movie.userId;
  const currentUserId = req.user.userId;
  
  if (movieUserId !== currentUserId) {
    return res.status(403).json({ error: 'Forbidden' });
  }
  next();
};
```

### 3. **Pool de Conexões**
Reutilização eficiente de conexões com PostgreSQL:
- Min connections: 5
- Max connections: 20
- Idle timeout: 30s

### 4. **AsyncHandler Wrapper**
Evitar repetição de try/catch em todos os handlers:

```typescript
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Uso
router.get('/movies', asyncHandler(async (req, res) => {
  const movies = await db.query('SELECT ...');
  res.json(movies);
}));
```

### 5. **Tratamento Centralizado de Erros**

```typescript
app.use((err, req, res, next) => {
  console.error(err);
  res.status(err.statusCode || 500).json({
    error: err.message,
    code: err.code
  });
});
```

## 🔧 Stack Utilizado

| Aspecto | Tecnologia |
|---------|-----------|
| Framework | Express.js |
| Linguagem | JavaScript/TypeScript |
| Database | PostgreSQL |
| Autenticação | JWT |
| Containerização | Docker |

## 📊 Endpoints Principais

```
POST   /auth/register           - Registrar novo usuário
POST   /auth/login              - Login
GET    /api/movies              - Listar filmes do usuário
POST   /api/movies              - Criar novo filme
GET    /api/movies/:id          - Buscar filme específico
PUT    /api/movies/:id          - Atualizar filme
DELETE /api/movies/:id          - Deletar filme
```

## 🚀 Como Rodar

```bash
# Clonar repositório
git clone https://github.com/usuario/movie-api.git
cd movie-api

# Instalar dependências
npm install

# Configurar ambiente
cp .env.example .env
# Adicionar: DATABASE_URL, JWT_SECRET

# Iniciar servidor
npm start

# Acesso: http://localhost:3000
```

## 🧪 Testes

```bash
# Testes unitários
npm run test

# Testes de integração
npm run test:integration

# Cobertura
npm run test:coverage
```

## 🎓 Principais Aprendizados

✅ **Segurança em APIs REST**  
✅ **Controle de concorrência no banco**  
✅ **Organização de código escalável**  
✅ **Autenticação e autorização**  
✅ **Pool de conexões otimizado**

## 📁 Estrutura de Pastas

```
src/
├── middleware/       # Autenticação e autorização
├── controllers/      # Handlers de requisições
├── models/           # Definições de dados
├── db/               # Configuração do banco
├── utils/            # Utilities
└── routes/           # Definição de rotas
```

---

## 📞 Contato

GitHub: [seu-usuario](https://github.com)

**Last Updated:** Março 2026
