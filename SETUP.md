# Link Manager — Setup no Supabase

## 1. Cria um novo projeto no Supabase

## 2. Vai ao SQL Editor e corre este código:

```sql
-- Utilizadores
create table users (
  id uuid default gen_random_uuid() primary key,
  username text unique not null,
  password_hash text not null,
  is_admin boolean default false,
  created_at timestamptz default now()
);

-- Categorias
create table categories (
  id serial primary key,
  name text not null,
  position int default 999
);

-- Links por categoria
create table links (
  id serial primary key,
  category_id int references categories(id) on delete cascade,
  title text not null,
  url text not null,
  description text default ''
);

-- Vista Pública (links que o modo Vista pode ver)
create table vista_publica (
  id serial primary key,
  title text not null,
  url text not null
);

-- Texto Público (notas partilhadas)
create table texto_publico (
  id serial primary key,
  content text not null,
  user_id uuid references users(id),
  updated_at timestamptz default now()
);
```

## 3. Desativa RLS em todas as tabelas
- Table Editor → cada tabela → RLS → Disable

## 4. Substitui as credenciais nos ficheiros HTML
Nos ficheiros index.html e app.html, substitui:
- `SUPABASE_URL_HERE` → o URL do teu projeto (ex: https://xxxx.supabase.co)
- `SUPABASE_KEY_HERE` → a tua anon/public key

## 5. Torna-te admin
Depois de criares a tua conta no site, vai ao Supabase:
Table Editor → users → edita a tua linha → is_admin = true

## 6. Deploy no Vercel
Coloca os 2 ficheiros (index.html e app.html) no teu projeto Vercel.
