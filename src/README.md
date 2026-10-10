# API Sistema de Autoatendimento para Restaurante

## 1. Descrição do projeto

API REST para um sistema de autoatendimento de restaurante. O objetivo é permitir que o cardápio (categorias e produtos) seja cadastrado e consultado de forma organizada, servindo de base para que clientes montem seus pedidos sem depender de um atendente.

O projeto foi desenvolvido na disciplina de Desenvolvimento Back-End (Engenharia de Software) e é evoluído individualmente na APS.

## 2. Identificação do estudante

**Lohanna Pereira dos Santos**

## 3. Tecnologias utilizadas

- Node.js
- TypeScript
- Express
- Supabase
- PostgreSQL
- Git / GitHub

## 4. Entidades e relacionamentos

### Category (categoria do cardápio)

| Atributo | Tipo | Descrição |
|---|---|---|
| id | UUID (PK) | Identificador |
| name | texto | Nome da categoria |
| description | texto | Descrição |
| icon | texto | Ícone |
| display_order | inteiro | Ordem de exibição |
| active | booleano | Se está ativa |

### Product (produto)

| Atributo | Tipo | Descrição |
|---|---|---|
| id | UUID (PK) | Identificador |
| name | texto | Nome do produto |
| description | texto | Descrição |
| price | numérico | Preço |
| category_id | UUID (FK) | Categoria a que pertence |

### Relacionamentos

- Uma **Category** possui vários **Products** (1:N), ligados por `products.category_id → categories.id`.

## 5. Estrutura do projeto

```
src/
├── config/        # Configuração do Supabase
├── controllers/   # Recebem as requisições e devolvem as respostas HTTP
├── models/        # Representação das entidades
├── repositories/  # Acesso e persistência dos dados no Supabase
├── routes/        # Definição dos endpoints
├── app.ts         # Configuração do Express e das rotas
└── server.ts      # Inicialização do servidor
```

## 6. Configuração e execução

```bash
git clone https://github.com/lohsantxs/projeto-sistema-restaura-sjp.git
cd projeto-sistema-restaura-sjp
npm install
copy .env.example .env
npm run dev
```

Edite o arquivo `.env` com os dados do seu projeto Supabase antes de rodar. O servidor sobe em `http://localhost:3000`.

Outros scripts: `npm run build` (compila para `dist/`) e `npm start` (executa a versão compilada).

## 7. Variáveis de ambiente

Crie um arquivo `.env` na raiz (veja o `.env.example`):

| Variável | Descrição |
|---|---|
| SUPABASE_URL | URL do projeto Supabase (`https://xxxx.supabase.co`) |
| SUPABASE_SECRET_KEY | Chave secreta do Supabase (nunca envie ao Git) |

O arquivo `.env` está no `.gitignore` e não é versionado.

## 8. Banco de dados

Execute no **SQL Editor** do Supabase:

```sql
create extension if not exists "pgcrypto";

create table categories (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  description text,
  icon text,
  display_order integer not null default 0,
  active boolean not null default true
);

create table products (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  description text,
  price numeric(10,2) not null,
  category_id uuid not null references categories(id)
);
```

## 9. Endpoints

### Categories

| Método | Endpoint | Descrição |
|---|---|---|
| GET | /categories | Lista todas as categorias |
| GET | /categories/:id | Consulta uma categoria pelo ID |
| GET | /categories/search/:keyword | Pesquisa por palavra-chave (nome ou descrição) |
| POST | /categories | Cadastra uma categoria |
| PUT | /categories/:id | Atualiza uma categoria |
| DELETE | /categories/:id | Remove uma categoria |

### Products

| Método | Endpoint | Descrição |
|---|---|---|
| GET | /products | Lista todos os produtos |
| GET | /products/:id | Consulta um produto pelo ID |
| POST | /products | Cadastra um produto |
| PUT | /products/:id | Atualiza um produto |
| DELETE | /products/:id | Remove um produto |

## 10. Exemplos de requisições

**POST /categories**

```json
{
  "name": "Pizzas",
  "description": "Pizzas tradicionais e especiais",
  "icon": "pizza",
  "display_order": 1,
  "active": true
}
```

**PUT /categories/:id**

```json
{
  "name": "Pizzas Especiais",
  "description": "Pizzas premium",
  "icon": "pizza",
  "display_order": 2,
  "active": true
}
```

**POST /products**

```json
{
  "name": "Pizza Margherita",
  "description": "Molho de tomate, mussarela e manjericão",
  "price": 39.9,
  "category_id": "UUID-DA-CATEGORIA"
}
```