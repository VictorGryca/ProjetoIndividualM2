# ClickVisit

Aplicação web para **agendamento de visitas a imóveis**. Corretores (autônomos ou vinculados a uma imobiliária) geram links de agendamento; clientes escolhem o melhor horário dentre a agenda do corretor e a disponibilidade do imóvel.

## Sumário

1. [](#c1)  
2. [](#c2)  
3. [](#c3)  
4. [](#c4)  
5. [](#c5)  


Tecnologias‑chave:

* **Node.js + Express** — back‑end em padrão MVC
* **PostgreSQL (Supabase)** — banco relacional
* **EJS** — views server‑side

---

## 1. Estrutura de pastas e arquivos

```text
.
├── assets/              # imagens e arquivos estáticos gerais
├── config/
│   └── db.js            # conexão PostgreSQL
├── controllers/         # lógica de negócio (por entidade)
├── documents/
│   └── PI-WAD.md        # documentação acadêmica
├── models/              # mapeamento das tabelas (DAO)
├── public/              # CSS, JS e imgs servidos pelo Express
├── routes/
│   ├── alunos.js        # exemplo de rota
│   └── frontRoutes.js   # rotas de views
├── scripts/
│   ├── ClickVisit.sql   # schema completo do banco
│   └── runSQLScript.js  # utilitário para popular o BD
├── tests/               # testes unitários e de integração
├── app.js               # app Express (rotas básicas)
├── server.js            # ponto de entrada — sobe o servidor
├── .env                 # variáveis de ambiente (DB, PORT…)
└── package.json         # dependências e scripts npm
```
```
meu-projeto/
│
├── config/                # Arquivos de configuração (ex: conexão com banco)
│   └── database.js
├── controllers/           # Lógica de controle das requisições
│   └── HomeController.js
├── models/                # Definição de modelos de dados (estrutura do banco)
│   └── User.js
├── routes/                # Definição das rotas do sistema
│   └── index.js
├── services/              # Serviços auxiliares do sistema
│   └── userService.js
├── assets/                # Arquivos públicos como imagens e fontes
├── scripts/               # Arquivos de JavaScript públicos
├── styles/                # Arquivos CSS públicos
├── tests/                 # Arquivos de testes unitários
│   └── example.test.js
├── .gitignore             # Arquivo para ignorar arquivos no Git
├── .env.example           # Arquivo de exemplo para variáveis de ambiente
├── jest.config.js         # Arquivo de configuração do Jest
├── package-lock.json      # Gerenciador de dependências do Node.js
├── package.json           # Gerenciador de dependências do Node.js
├── readme.md              # Documentação do projeto (Markdown)
├── server.js              # Arquivo principal que inicializa o servidor
└── rest.http              # Teste de endpoints (opcional)

```

## 2. Como executar o projeto localmente

1. **Clone o repositório**

   ```bash
   git clone https://github.com/<seu-user>/<seu-repo>.git
   cd <seu-repo>
   ```

2. **Instale as dependências**

   ```bash
   npm install
   ```

3. **Configure o ambiente**

   Copie `.env` e preencha:

   ```env
   DB_HOST=<host>
   DB_PORT=5432
   DB_USER=<usuario>
   DB_PASSWORD=<senha>
   DB_DATABASE=<nome_bd>
   PORT=3000
   ```

4. **Inicialize o banco**

   ```bash
   npm run init-db            # executa scripts/ClickVisit.sql
   ```

5. **Inicie em modo de desenvolvimento**

   ```bash
   npm run dev                # usa nodemon p/ hot-reload
   ```

   A aplicação estará em **[http://localhost:3000](http://localhost:3000)**.


## 3. Visão geral do sistema

| Papel           | Permissões principais                                                              |
| --------------- | ---------------------------------------------------------------------------------- |
| **Imobiliária** | Cadastro de imóveis, vinculação de corretores, bloqueio de horários (reforma etc.) |
| **Corretor**    | Mantém agenda pessoal, gera link exclusivo para o cliente                          |
| **Cliente**     | Acessa link, compara agendas, agenda visita                                        |

Fluxo resumido:

1. Corretor envia link do imóvel.
2. Cliente escolhe horário livre.
3. Sistema grava em `events` e `visits`.
4. Horário fica bloqueado para novos agendamentos.

---

