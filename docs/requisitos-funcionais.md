# Requisitos Funcionais

> Requisitos funcionais do Church Media Scheduler

## Visão Geral

Este documento lista os requisitos funcionais do sistema, transferidos do documento de visão do projeto. Para detalhes de implementação, consulte as issues no GitHub.

## Requisitos Funcionais

| ID | Descrição | Prioridade | Complexidade | Issues GitHub |
|----|-----------|------------|--------------|---------------|
| RQ1 | Gestor gerencia colaborador | Alta | Baixa | #1, #8, #9 |
| RQ2 | Gestor gerencia cultos | Alta | Baixa | #2, #12, #16 |
| RQ3 | Gestor gera escala | Alta | Alta | #3, #10, #11 |
| RQ4 | Gestor aprova escala (manda invite na agenda) | Média | Alta | #5, #15 |
| RQ5 | Colaborador visualiza a escala | Alta | Baixa | #7, #13, #14 |
| RQ6 | Gestor gerencia setores | Baixa | Média | #6 |
| RQ7 | Colaborador faz login | Baixa | Média | #18 |
| RQ8 | Gestor edita escala | Média | Média | - |

### Legenda

**Prioridade**:
- **Alta**: Essencial para o funcionamento do sistema
- **Média**: Importante mas não bloqueante
- **Baixa**: Desejável, pode ser implementado posteriormente

**Complexidade**:
- **Baixa**: CRUD simples, implementação direta
- **Média**: Requer lógica de negócio moderada
- **Alta**: Algoritmos complexos, múltiplas integrações

## Descrição dos Requisitos

### RQ1 - Gestor Gerencia Colaborador

O gestor deve poder cadastrar, visualizar, editar e excluir colaboradores do sistema.

**Informações gerenciadas**:
- Nome
- Email
- WhatsApp
- CPF
- Setores participantes
- Tipo: Ativo ou Reserva
- Horários de disponibilidade

### RQ2 - Gestor Gerencia Cultos

O gestor deve poder cadastrar, visualizar, editar e excluir cultos da igreja.

**Informações gerenciadas**:
- Dia da semana
- Horário

### RQ3 - Gestor Gera Escala

O gestor solicita a geração automática da escala mensal. O sistema deve alocar colaboradores aos cultos respeitando todas as regras de negócio.

**Considerações**:
- Disponibilidade dos colaboradores
- Setores de atuação
- Distribuição equitativa mensal
- Evitar repetição de horários entre semanas
- Cobertura total de todos os cultos

### RQ4 - Gestor Aprova Escala

Após revisar a escala gerada, o gestor a aprova. O sistema então envia automaticamente convites de calendário por email para todos os colaboradores alocados.

**Funcionalidades**:
- Aprovação da escala gerada
- Envio automático de emails com convites de calendário
- Em caso de edição, notificar apenas os colaboradores afetados

### RQ5 - Colaborador Visualiza Escala

Colaboradores autenticados podem acessar o sistema para visualizar quando estão escalados.

**Funcionalidades**:
- Visualização das próprias alocações
- Filtro por período
- Exportação para calendário

### RQ6 - Gestor Gerencia Setores

O gestor deve poder cadastrar, visualizar, editar e excluir setores de atuação.

**Informações gerenciadas**:
- Nome do setor
- Descrição (opcional)

### RQ7 - Colaborador Faz Login

Sistema de autenticação para controle de acesso.

**Funcionalidades**:
- Login com usuário e senha
- Controle de acesso por papel (Gestor ou Colaborador)
- Logout

### RQ8 - Gestor Edita Escala

O gestor pode fazer ajustes manuais em escalas já geradas ou aprovadas.

**Funcionalidades**:
- Trocar colaborador em uma alocação
- Adicionar ou remover alocações
- Notificar colaboradores afetados pelas mudanças

## Requisitos Não Funcionais

| ID | Descrição | Prioridade | Complexidade |
|----|-----------|------------|--------------|
| RN01 | O sistema deve permitir pelo menos 100 usuários simultâneos | Média | Média |
| RN02 | O serviço deve estar disponível pelo menos de 6h às 22h | Alta | Baixa |
| RN03 | O sistema deve funcionar em navegadores Google Chrome, Opera GX e Safari | Alta | Baixa |
| RN04 | O serviço deve estar em conformidade com as leis de proteção de dados (LGPD) | Alta | Alta |

