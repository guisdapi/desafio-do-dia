# 🚀 Desafio do Dia | Plataforma Gamificada de Ensino

![Status](https://img.shields.io/badge/Status-Em%20Produção-success)
![React](https://img.shields.io/badge/Frontend-React%20%7C%20Vite-61DAFB?logo=react&logoColor=black)
![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688?logo=fastapi&logoColor=white)
![MongoDB](https://img.shields.io/badge/Database-MongoDB-47A248?logo=mongodb&logoColor=white)

> **Acesse a aplicação ao vivo:** [Link do Deploy na Vercel](https://project2-2026b-guisdapi.vercel.app/)

## 📌 Sobre o Projeto
O **Desafio do Dia** é uma Single Page Application (SPA) fullstack construída para engajar alunos fora do ambiente de sala de aula. A plataforma permite que professores publiquem desafios diários, enquanto os alunos respondem para manter uma ofensiva (*streak*) e acumular experiência (XP). 

Com uma estética imersiva *Cyber/Y2K*, o sistema conta com separação de privilégios (Alunos vs. Professores), cálculos de taxa de acerto em tempo real e um ecossistema seguro de rotas.

## ⚙️ Arquitetura e Decisões de Engenharia

Este projeto foi arquitetado separando totalmente as responsabilidades de Frontend e Backend, focando em escalabilidade, segurança e performance.

- **Autenticação Stateless com JWT:** Senhas e PINs protegidos por `bcrypt`. O acesso é gerenciado via JSON Web Tokens, onde o payload do token já carrega o `role` (aluno/professor) e o ID da turma, eliminando a necessidade de consultas redundantes ao banco a cada requisição.
- **Padrão BFF (Backend For Frontend) & Otimização:** Para evitar o clássico problema de *N+1 queries*, a listagem de submissões busca e cruza os dados dos alunos diretamente em memória (O(1)) no backend antes de entregar a resposta limpa para o front.
- **Segurança de Dados e Exclusão em Cascata:** Prevenção de acessos indevidos (Erro 403) na validação de propriedade da turma e sistema de *cascade delete* automatizado: ao excluir uma turma, o backend limpa desafios, visualizações e submissões vinculadas, evitando dados órfãos no banco.
- **Reatividade Híbrida:** A interface do aluno reage a desafios do passado criando bloqueios automáticos no cliente (`isReadOnly`), enquanto validações críticas são rigorosamente garantidas pelo `Pydantic` no servidor.

## 🛠️ Stack Tecnológica

**Frontend:**
- React + Vite
- Tailwind CSS (Estilização com variáveis CSS dinâmicas)
- Axios (Interceptadores globais para injeção de JWT e tratamento de erros 401)
- React Router DOM
- Deploy: Vercel

**Backend:**
- Python + FastAPI
- MongoDB + Motor (Driver 100% assíncrono para NoSQL)
- Pydantic (Validação de Schemas de entrada e saída)
- Deploy: Render

## 🚀 Como Rodar o Projeto Localmente

### Pré-requisitos
- Node.js (v18+)
- Python (3.10+)
- Uma conta no MongoDB Atlas (ou instância local do MongoDB)

### 1. Backend (FastAPI)
```bash
# Clone o repositório
git clone [https://github.com/guisdapi/desafio-do-dia.git](https://github.com/guisdapi/desafio-do-dia.git)

# Entre na pasta do backend
cd backend

# Crie e ative o ambiente virtual
python -m venv venv
source venv/bin/activate  # No Windows: venv\Scripts\activate

# Instale as dependências
pip install -r requirements.txt

# Crie um arquivo .env e adicione sua string de conexão do MongoDB
# MONGO_URI=mongodb+srv://...

# Inicie o servidor
uvicorn main:app --reload
```
A API estará rodando em `http://localhost:8000`. A documentação interativa (Swagger) pode ser acessada em `http://localhost:8000/docs`.

### 2. Frontend (React)
```bash
# Entre na pasta do frontend
cd frontend

# Instale as dependências
npm install

# Crie um arquivo .env com a URL da API
# VITE_API_URL=http://localhost:8000

# Inicie a aplicação
npm run dev
```
A interface estará disponível em `http://localhost:5173`.

---
*Projeto idealizado por Guilherme Serafini Dapieve.*