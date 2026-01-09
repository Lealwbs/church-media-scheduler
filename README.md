# Church Media Scheduler



> Sistema de gestão e alocação automática de colaboradores em tarefas da igreja

## Autores

- [RenatoMAP77](https://github.com/RenatoMAP77)
- [Lealwbs](https://github.com/Lealwbs)

## Índice

- [Sobre o Projeto](#sobre-o-projeto)
- [Funcionalidades](#funcionalidades)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Arquitetura](#arquitetura)
- [Instalação](#instalação)
- [Uso](#uso)
- [Documentação](#documentação)
- [Contribuindo](#contribuindo)

## Sobre o Projeto

O **Church Media Scheduler** é uma aplicação web desenvolvida para automatizar e otimizar a geração de escalas da equipe de mídia da igreja. O sistema considera múltiplos fatores como disponibilidade dos colaboradores, funções específicas, setores de atuação e distribuição equilibrada dos serviços ao longo do mês.

### Problema

Igrejas frequentemente enfrentam desafios na organização manual de escalas:
- Dificuldade em considerar a disponibilidade de todos os colaboradores
- Risco de sobrecarregar alguns membros enquanto outros ficam ociosos
- Complexidade em evitar repetições de horários para os mesmos colaboradores
- Tempo gasto na comunicação e notificação de cada pessoa
- Falta de histórico e controle centralizado das escalas

### Solução

O Church Media Scheduler automatiza todo esse processo através de:
- **Algoritmo inteligente de alocação** que considera múltiplas restrições
- **Gestão centralizada** de colaboradores, cultos e setores
- **Distribuição equitativa** de serviços mensais
- **Notificações automáticas** por email com convites de calendário
- **Interface intuitiva** para gestores e colaboradores

## Funcionalidades

### Para Gestores

- **Gestão de Colaboradores** (RQ1)
  - Cadastro completo com dados pessoais (nome, email, WhatsApp, CPF)
  - Definição de horários de disponibilidade
  - Configuração de setores de atuação (Som, Projeção, etc.)
  - Status: Ativo ou Reserva

- **Gestão de Cultos** (RQ2)
  - Cadastro de cultos com dia da semana e horário
  - Visualização e edição de cultos existentes
  - Remoção de cultos

- **Gestão de Setores** (RQ6)
  - Criação e gerenciamento de setores
  - Definição de nomes e descrições

- **Geração de Escalas** (RQ3)
  - Geração automática considerando:
    - Disponibilidade dos colaboradores
    - Evitar repetição de horários semanais
    - Distribuição equitativa mensal
    - Cobertura total de todos os cultos
    - Separação por setores
  - Visualização da escala gerada

- **Aprovação de Escalas** (RQ4)
  - Revisão da escala gerada
  - Aprovação e envio automático de emails
  - Notificação apenas dos envolvidos em caso de atualização

### Para Colaboradores

- **Autenticação** (RQ7)
  - Login seguro com usuário e senha
  - Acesso personalizado ao sistema

- **Visualização de Escalas** (RQ5)
  - Consulta da escala aprovada
  - Visualização dos dias e horários alocados
  - Acesso via web

## Tecnologias Utilizadas

### Backend
- Node.js 
- API RESTful
- Banco de dados relacional (PostgreSQL/MySQL)
- Sistema de autenticação JWT

### Frontend
- React

### Infraestrutura
- Docker 
- Serviço de email (SMTP/SendGrid)
- Geração de convites de calendário (google calendar)
- Hospedagem cloud

## Arquitetura

![Diagrama arquitetural](./docs/DiagramaArquitetural.png)

O sistema segue uma arquitetura em camadas:

```
ATUALIZAR ESSE POR UM MELHOR
┌─────────────────────────────────────┐
│         Frontend (Web App)          │
├─────────────────────────────────────┤
│            API REST                 │
├─────────────────────────────────────┤
│       Camada de Serviços            │
│  - GeradorEscalaService             │
│  - NotificacaoService               │
│  - AutenticacaoService              │
├─────────────────────────────────────┤
│      Camada de Domínio              │
│  - Colaborador                      │
│  - Culto                            │
│  - Escala                           │
│  - Alocacao                         │
│  - Setor                            │
├─────────────────────────────────────┤
│    Camada de Persistência           │
│         (Banco de Dados)            │
└─────────────────────────────────────┘
```

### Diagrama de Classes UML

Consulte o diagrama completo em [docs/uml.puml](./docs/uml.puml).

**Principais entidades:**
- **Colaborador**: Representa os membros da equipe
- **Culto**: Define os eventos que precisam de cobertura
- **Setor**: Categoriza as funções (Som, Projeção, etc.)
- **Escala**: Agrupa todas as alocações de um período
- **Alocacao**: Relaciona colaborador, culto e setor em uma data específica
- **Usuario**: Credenciais de acesso ao sistema

## Instalação

### Pré-requisitos

- Node.js 18+ 
- Banco de dados (PostgreSQL 14+ ou MySQL 8+)
- Servidor SMTP ou conta SendGrid

### Passos

1. Clone o repositório:
```bash
git clone https://github.com/Lealwbs/church-media-scheduler.git
cd church-media-scheduler
```

2. Instale as dependências:
```bash
cd code
npm install
```

3. Configure as variáveis de ambiente:
```bash
cp .env.example .env
# Edite o arquivo .env com suas configurações
```

4. Execute as migrações do banco de dados:
```bash
npm run migrate
```

5. Inicie o servidor:
```bash
npm run dev
```

Para instruções detalhadas, consulte o [Guia de Instalação](./docs/instalacao.md).

## Uso

### Configuração Inicial

1. Acesse o sistema como gestor
2. Cadastre os setores (Som, Projeção, etc.)
3. Cadastre os cultos (dias e horários)
4. Cadastre os colaboradores com suas disponibilidades

### Geração de Escala

1. Acesse a seção "Gerar Escala"
2. Selecione o mês desejado
3. Clique em "Gerar"
4. Revise a escala gerada
5. Aprove e envie as notificações

### Visualização (Colaborador)

1. Faça login com suas credenciais
2. Acesse "Minha Escala"
3. Visualize seus dias e horários alocados

## Documentação

### Documentos Principais
- [Requisitos Funcionais](./docs/requisitos-funcionais.md) - Lista de requisitos funcionais e não funcionais
- [Regras de Negócio](./docs/regras-negocio.md) - Regras do algoritmo de geração de escala
- [Diagrama UML](./docs/uml.puml) - Diagrama de classes do sistema

### Arquitetura e Desenvolvimento
- [Guia de Instalação](./docs/instalacao.md) - Instruções de setup
- [API Documentation](./docs/api.md) - Documentação dos endpoints (IMPLEMENTAR SWAGGER)

## Licença

Distribuído sob a licença MIT. Veja [LICENSE](./LICENSE) para mais informações.

---

