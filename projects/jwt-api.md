# API com JWT - Autenticação e Autorização

Sistema de API implementando autenticação e autorização com JWT, demonstrando boas práticas de segurança em profundidade.

## 📋 Visão Geral

**Problema Real:**
Qualquer API precisa de autenticação robusta. Má implementação leva a vazamento de dados e acesso não autorizado.

**Solução:**
Um sistema de autenticação baseado em JWT com refresh tokens, rate limiting e proteção contra ataques comuns.

## 🏗️ Fluxo de Autenticação

```
┌─────────────┐
│   Cliente   │
└──────┬──────┘
       │
       │ 1. POST /auth/login
       ▼
┌──────────────────────────┐
│    Validar Credenciais   │  ← Hash de senha
└──────┬───────────────────┘
       │
       │ 2. Retorno
       ▼
┌──────────────────────────────────────────┐
│  Access Token (JWT - 15min)              │
│  Refresh Token (JWT - 7 dias, httpOnly)  │
└──────┬───────────────────────────────────┘
       │
       │ 3. Requisições subsequentes
       ▼
┌──────────────────────────┐
│  Validar Access Token    │  ← Verificar assinatura
└──────┬───────────────────┘
       │
       ├─ Válido ─────> Continuar
       │
       └─ Expirado → Usar Refresh Token
```

## 🎯 Principais Decisões Técnicas

### 1. **JWT com Secrets Seguros**

```typescript
const token = jwt.sign(
  {
    userId: user.id,
    email: user.email,
    role: user.role,
    iat: nowSeconds,
    exp: nowSeconds + 900 // 15 minutos
  },
  process.env.JWT_SECRET,
  { algorithm: 'HS256' }
);
```

### 2. **Refresh Token Pattern**

```typescript
// Access token vence rápido (15 min)
// Refresh token tem duração longa (7 dias)
// Refresh token é armazenado no banco (pode ser revogado)

const newAccessToken = jwt.sign(payload, SECRET, { expiresIn: '15m' });
const refreshToken = jwt.sign(payload, REFRESH_SECRET, { expiresIn: '7d' });

// Armazenar refresh token hash no DB para revogação
await db.refreshTokens.create({
  userId: user.id,
  tokenHash: hashToken(refreshToken),
  expiresAt: futureDate,
  revokedAt: null
});
```

### 3. **RBAC - Role-Based Access Control**

```typescript
// Middleware para verificar roles
const requireRole = (...roles: string[]) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Insufficient permissions' });
    }
    next();
  };
};

// Uso
router.delete('/api/users/:id', requireRole('admin'), deleteUserHandler);
```

### 4. **Hashing Seguro de Senhas**

```typescript
import bcrypt from 'bcrypt';

// Criar usuário
const hashedPassword = await bcrypt.hash(password, 10); // salt rounds
await user.save({ password: hashedPassword });

// Validar login
const isPasswordValid = await bcrypt.compare(password, user.password);
```

### 5. **Rate Limiting**

```typescript
import rateLimit from 'express-rate-limit';

const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 5, // 5 tentativas por IP
  message: 'Muitas tentativas de login. Tente novamente mais tarde.',
  standardHeaders: true,
  legacyHeaders: false,
});

router.post('/auth/login', loginLimiter, loginHandler);
```

## 🔧 Stack Utilizado

| Aspecto | Tecnologia |
|---------|-----------|
| Framework | Express.js |
| Autenticação | JWT (jsonwebtoken) |
| Hashing | bcrypt |
| Validação | Zod |
| Rate Limiting | express-rate-limit |
| Database | PostgreSQL |
| Containerização | Docker |

## 📊 Endpoints Principais

```
POST   /auth/signup          - Registrar novo usuário
POST   /auth/login           - Login com credenciais
POST   /auth/refresh         - Renovar access token
POST   /auth/logout          - Revogar refresh token

GET    /api/me               - Dados do usuário autenticado
PUT    /api/me               - Atualizar perfil
POST   /api/me/password      - Mudar senha

GET    /api/users            - Admin: listar usuários
DELETE /api/users/:id        - Admin: deletar usuário
```

## 🚀 Como Rodar

```bash
# Instalar dependências
npm install

# Configurar variáveis
cp .env.example .env
# Definir: JWT_SECRET, REFRESH_SECRET, DATABASE_URL

# Rodar migrations
npm run db:migrate

# Iniciar
npm run dev
```

## 📝 Exemplo de Flow

```bash
# 1. Registrar usuário
curl -X POST http://localhost:3000/auth/signup \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePass123!",
    "name": "João"
  }'

# 2. Login
curl -X POST http://localhost:3000/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePass123!"
  }'
# Retorna: { accessToken, refreshToken }

# 3. Usar token em requisições
curl -X GET http://localhost:3000/api/me \
  -H "Authorization: Bearer <accessToken>"

# 4. Renovar token quando expirar
curl -X POST http://localhost:3000/auth/refresh \
  -H "Content-Type: application/json" \
  -d '{ "refreshToken": "<refreshToken>" }'
```

## 🛡️ Proteções contra Ataques

| Ataque | Proteção |
|--------|----------|
| Brute Force | Rate limiting + account lockout |
| SQL Injection | Prepared statements |
| XSS | Sanitização + CORS |
| CSRF | CSRF tokens para estado-mutantes |
| Token Hijacking | HttpOnly cookies + HTTPS |
| Senha fraca | Validação de força de senha |

## 🎓 Principais Aprendizados

✅ **Segurança em Profundidade:** Múltiplas camadas de proteção  
✅ **JWT Best Practices:** Curta duração + refresh tokens  
✅ **RBAC:** Controle granular de permissões  
✅ **Hashing:** Importância de usar bcrypt (não MD5/SHA1)  
✅ **Rate Limiting:** Proteção contra força bruta

---

## ⚠️ Checklist de Segurança

- ✅ Senhas com bcrypt (minimum 10 rounds)
- ✅ Access tokens com 15-30 min de expiração
- ✅ Refresh tokens em httpOnly cookies
- ✅ Rate limiting em endpoints sensíveis
- ✅ Logs de tentativas de acesso
- ✅ Revogação de tokens no logout
- ✅ HTTPS obrigatório em produção
- ✅ Validação de entrada

---

## 📞 Contato

GitHub: [seu-usuario](https://github.com)

**Last Updated:** Março 2026
