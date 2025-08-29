
# Guia de Alterações com Prisma ORM

Este guia descreve o fluxo de trabalho para **alterar o schema do Prisma**, criar **migrations** e aplicar mudanças no banco de dados em um projeto com **Next.js + Prisma**.

---

## 📂 Estrutura do Prisma no Projeto

```
/prisma
  ├── schema.prisma   # Definição do schema
  └── migrations/     # Histórico das migrations
```

---

## 🚀 Passo a passo para alterar tabelas

### 1. Editar o `schema.prisma`
Exemplo: adicionar um campo `name` ao modelo `User`.

Antes:
```prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  password  String
  createdAt DateTime @default(now())
}
```

Depois:
```prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  password  String
  name      String?
  createdAt DateTime @default(now())
}
```

---

### 2. Criar uma nova migration
Rodar o comando:
```bash
npx prisma migrate dev --name add-user-name
```

Isso gera uma pasta dentro de `prisma/migrations/` com SQL correspondente à alteração.

---

### 3. Aplicar migration no banco
O comando acima já aplica no banco local.  
Se outro dev puxar a alteração, basta rodar:

```bash
npx prisma migrate dev
```

---

### 4. Regenerar o cliente Prisma
Sempre que alterar o schema, rode:
```bash
npx prisma generate
```

Isso atualiza os tipos TypeScript do Prisma no projeto.

---

## 🧑‍🤝‍🧑 Fluxo em equipe

1. Um dev altera o `schema.prisma`.
2. Cria uma migration com `npx prisma migrate dev --name <nome>`.
3. Comita **schema.prisma** + pasta `migrations/`.
4. Outro dev dá `git pull`, e roda:
   ```bash
   npx prisma migrate dev
   npx prisma generate
   ```

---

## ✅ Boas práticas de nomenclatura de migrations

- `init` → primeira migration.  
- `add-user-name` → adicionar coluna `name` em `User`.  
- `create-product-table` → criar nova tabela `Product`.  
- `update-order-status` → atualizar coluna `status` da tabela `Order`.  

Use nomes **curtos e descritivos**.

---

## 📌 Resumo dos comandos principais

- **Gerar migration:**  
  ```bash
  npx prisma migrate dev --name <nome>
  ```

- **Aplicar migrations existentes:**  
  ```bash
  npx prisma migrate dev
  ```

- **Gerar cliente Prisma:**  
  ```bash
  npx prisma generate
  ```
