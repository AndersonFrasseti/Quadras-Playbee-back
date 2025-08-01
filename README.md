# Quadras Playbee - Backend

Este é o backend da aplicação Quadras Playbee, uma API RESTful para gerenciamento de quadras e reservas.

## 📜 Sobre

O Quadras Playbee é um sistema de agendamento de quadras esportivas, permitindo que usuários (jogadores e administradores) visualizem, reservem e gerenciem horários em diferentes quadras. Esta API fornece todos os endpoints necessários para a funcionalidade do sistema.

## ✨ Features

  * **Autenticação de Usuário**: Sistema de login seguro com JSON Web Tokens (JWT).
  * **Gerenciamento de Usuários**:
      * Criação de novos usuários (jogadores ou administradores).
      * Atualização de informações de perfil.
      * Exclusão de usuários.
      * Listagem de todos os usuários (apenas para administradores).
  * **Gerenciamento de Quadras**:
      * Cadastro, edição e exclusão de quadras (apenas para administradores).
      * Listagem de todas as quadras disponíveis.
  * **Sistema de Reservas**:
      * Criação de novas reservas, com validação de conflitos de horário.
      * Cancelamento de reservas.
      * Atualização de informações da reserva.
      * Listagem de todas as reservas (para administradores) ou apenas as do usuário autenticado.

## 🛠️ Tecnologias Utilizadas

Este projeto foi construído com as seguintes tecnologias:

  * **[Node.js](https://nodejs.org/)** - Ambiente de execução JavaScript.
  * **[TypeScript](https://www.typescriptlang.org/)** - Superset do JavaScript que adiciona tipagem estática.
  * **[Fastify](https://www.fastify.io/)** - Framework web rápido e de baixa sobrecarga.
  * **[Prisma](https://www.prisma.io/)** - ORM para Node.js e TypeScript.
  * **[PostgreSQL](https://www.postgresql.org/)** - Banco de dados relacional.
  * **[Zod](https://zod.dev/)** - Biblioteca de validação de esquemas.
  * **[Docker](https://www.docker.com/)** - Plataforma de containerização para fácil deploy.
  * **[JWT (JSON Web Token)](https://jwt.io/)** - Para autenticação segura.

## 🚀 Começando

Siga estas instruções para configurar e rodar o projeto em seu ambiente local.

### Pré-requisitos

  * Node.js (versão 18 ou superior)
  * Docker e Docker Compose
  * Um cliente de banco de dados PostgreSQL

### Instalação

1.  **Clone o repositório:**

    ```bash
    git clone https://github.com/andersonfrasseti/quadras-playbee-back.git
    cd quadras-playbee-back
    ```

2.  **Instale as dependências:**

    ```bash
    npm install
    ```

3.  **Configure as variáveis de ambiente:**

    Crie um arquivo `.env` na raiz do projeto, baseado no exemplo a seguir:

    ```env
    DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DATABASE"
    JWT_SECRET="sua-chave-secreta-super-segura"
    PORT=3003
    ```

4.  **Execute as migrações do banco de dados:**

    O Prisma utilizará a `DATABASE_URL` para aplicar as migrações e criar as tabelas necessárias.

    ```bash
    npx prisma migrate dev
    ```

### Rodando a Aplicação

Para iniciar o servidor em modo de desenvolvimento (com hot-reload), execute:

```bash
npm run dev
```

A API estará disponível em `http://localhost:3003`.

## 🐳 Rodando com Docker

Para construir e iniciar o container Docker, siga os passos:

1.  **Construa a imagem Docker:**

    ```bash
    docker build -t quadras-playbee-backend .
    ```

2.  **Inicie o container:**

    Certifique-se de que o seu banco de dados PostgreSQL esteja acessível a partir do container.

    ```bash
    docker run -d --name playbee-backend -p 3003:3003 \
      -e DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DATABASE" \
      -e JWT_SECRET="sua-chave-secreta-super-segura" \
      -e PORT="3003" \
      --restart unless-stopped \
      quadras-playbee-backend
    ```

## 📝 Endpoints da API

A coleção de requisições do Insomnia ou Postman pode ser importada dos arquivos `.http` encontrados em:

  * `src/http/controllers/users/@user.http`
  * `src/http/controllers/courts/@court.http`
  * `src/http/controllers/schedule/@schedule.http`

### Autenticação

  * `POST /auth/login` - Autentica um usuário e retorna um token JWT.
  * `POST /auth/logout` - Realiza o logout do usuário.

### Usuários

  * `POST /users` - Cria um novo usuário.
  * `GET /users` - Lista todos os usuários.
  * `GET /users/:id` - Obtém um usuário por ID.
  * `PUT /users/:id` - Atualiza um usuário.
  * `DELETE /users/:id` - Deleta um usuário.

### Quadras (`/court`)

  * `POST /court` - Cria uma nova quadra.
  * `GET /court` - Lista todas as quadras.
  * `GET /court/:id` - Obtém uma quadra por ID.
  * `PUT /court/:id` - Atualiza uma quadra.
  * `DELETE /court/:id` - Deleta uma quadra.

### Reservas (`/schedule`)

  * `POST /schedule` - Cria uma nova reserva.
  * `GET /schedule` - Lista todas as reservas (ou as do usuário).
  * `GET /schedule/:id` - Obtém uma reserva por ID.
  * `PUT /schedule/:id` - Atualiza uma reserva.
  * `PATCH /schedule/:id/cancel` - Cancela uma reserva.
  * `DELETE /schedule/:id` - Deleta uma reserva.

## 🗃️ Banco de Dados

O esquema do banco de dados é gerenciado pelo Prisma e pode ser encontrado em `prisma/schema.prisma`. As tabelas principais são:

  * **User**: Armazena informações dos usuários.
  * **Court**: Armazena informações das quadras.
  * **Schedule**: Armazena as reservas, relacionando um usuário a uma quadra em um determinado período.
