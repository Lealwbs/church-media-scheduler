# Regras de Negócio

> Regras de negócio do algoritmo de geração de escala

## Visão Geral

Este documento descreve as regras de negócio que o sistema deve seguir, especialmente no algoritmo de geração automática de escala mensal.

## Regras do Algoritmo de Geração de Escala

O algoritmo de geração de escala deve considerar as seguintes restrições ao alocar colaboradores aos cultos:

### 1. Evitar Repetição de Horários

Colaboradores não devem servir no mesmo horário em semanas consecutivas.

**Exemplo**:
- ✅ **Aceitável**: João serviu domingo 18h semana 1, pode servir domingo 20h semana 2
- ❌ **Não aceitável**: João serviu domingo 18h semana 1, NÃO deve servir domingo 18h semana 2

**Implementação**:
- Ao alocar um colaborador, verificar suas alocações nas últimas 7 dias
- Se houver alocação no mesmo dia da semana + mesmo horário, pular para outro colaborador

### 2. Distribuição Equitativa

Todos os colaboradores ativos devem ter aproximadamente o mesmo número de alocações no mês.

**Exemplo**:
- ✅ **Aceitável**: João 9x, Maria 9x, Pedro 10x (diferença máxima de 1-2)
- ❌ **Não aceitável**: João 15x, Maria 5x, Pedro 0x

**Implementação**:
- Manter contador de alocações por colaborador no mês
- Priorizar colaboradores com menos alocações
- Tolerar diferença máxima de 2 alocações entre o mais alocado e o menos alocado

### 3. Cobertura Total

Todos os cultos devem ter todos os setores preenchidos. Não pode haver horários sem colaboradores.

**Exemplo**:
- ✅ **Aceitável**: Domingo 18h → Som: João, Projeção: Maria
- ❌ **Não aceitável**: Domingo 18h → Som: João, Projeção: [vazio]

**Implementação**:
- Para cada culto em cada data do mês
- Para cada setor ativo
- Deve haver exatamente 1 colaborador alocado

### 4. Respeitar Setores

Cada culto precisa de cobertura em cada setor cadastrado. Colaboradores só podem ser alocados para setores em que atuam.

**Exemplo**:
- ✅ **Aceitável**: João (atua em Som) → alocado para Som
- ❌ **Não aceitável**: João (atua apenas em Som) → alocado para Projeção

**Implementação**:
- Filtrar apenas colaboradores que possuem o setor específico na lista de setores de atuação
- Um colaborador pode atuar em múltiplos setores

### 5. Disponibilidade

Colaboradores só podem ser alocados em horários que declararam disponibilidade.

**Exemplo**:
- João declarou disponibilidade: Domingo 18h-20h
- ✅ **Aceitável**: Alocar João para culto de domingo 18h ou 19h
- ❌ **Não aceitável**: Alocar João para culto de domingo 20h30 ou segunda 19h

**Implementação**:
- Verificar se o colaborador tem horário de disponibilidade que:
  - Corresponde ao dia da semana do culto
  - O horário do culto está dentro do intervalo (horarioInicio <= horarioCulto <= horarioFim)

### 6. Priorização

Colaboradores "Ativos" têm prioridade sobre "Reservas". Reservas não deverão ser usados.

**Ordem de prioridade**:
1. Colaboradores ATIVOS
2. Colaboradores RESERVA (apenas se não houver ativos disponíveis)

**Implementação**:
- Ordenar lista de colaboradores elegíveis por tipo (ATIVO primeiro)
- Tentar primeiro todos os ativos
- Só recorrer a reservas se impossível completar com ativos

## Algoritmo Sugerido

### Pseudocódigo

```
FUNÇÃO gerarEscala(mes, ano):
    // 1. Preparação
    cultos = buscarCultosAtivos()
    colaboradores = buscarColaboradoresAtivos()
    setores = buscarSetoresAtivos()
    datas = gerarDatasDoMes(mes, ano, cultos)

    escala = novaEscala(mes, ano)
    contadorAlocacoes = {} // contador por colaborador

    // 2. Para cada data/culto
    PARA CADA (data, culto) EM datas:
        PARA CADA setor EM setores:
            // Filtrar colaboradores elegíveis
            elegiveis = colaboradores.filtrar(c =>
                c.atuaNoSetor(setor) E
                c.disponivelEm(culto.diaSemana, culto.horario) E
                NAO c.serviuNoMesmoHorarioNaUltimaSemana(culto, escala, data)
            )

            // Ordenar por prioridade
            elegiveis.ordenarPor(
                1. tipo (ATIVO > RESERVA)
                2. contadorAlocacoes[colaborador] (menor primeiro)
            )

            // Selecionar primeiro da lista
            SE elegiveis.vazio():
                RETORNAR erro("Impossível gerar escala completa")

            selecionado = elegiveis[0]

            // Criar alocação
            alocacao = novaAlocacao(data, culto, setor, selecionado)
            escala.adicionarAlocacao(alocacao)
            contadorAlocacoes[selecionado]++

    // 3. Validação
    SE NAO escala.validarCobertura():
        RETORNAR erro("Escala incompleta")

    SE NAO escala.validarDistribuicao():
        AVISAR("Distribuição não está perfeitamente equilibrada")

    RETORNAR escala
```

### Validações Pós-Geração

1. **Cobertura Completa**:
   - Verificar se todas as combinações (data, culto, setor) têm alocação

2. **Distribuição Equitativa**:
   - Calcular desvio padrão das alocações
   - Se diferença entre max e min > 2, emitir aviso

3. **Restrições Respeitadas**:
   - Verificar disponibilidades
   - Verificar setores
   - Verificar repetições

## Casos Especiais

### Indisponibilidades Temporárias (Futuro)

Colaboradores podem ter indisponibilidades temporárias (ex: viagem, compromisso).

**Regra**: Considerar como "não disponível" naquelas datas específicas, mesmo que o horário normal esteja na disponibilidade fixa.

### Falta de Colaboradores

Se impossível gerar escala completa:
1. Identificar o gargalo (setor, horário)
2. Retornar erro descritivo
3. Sugerir ações:
   - Cadastrar mais colaboradores
   - Revisar disponibilidades
   - Considerar usar mais colaboradores RESERVA
   - Desativar cultos temporariamente

### Edições Manuais

Quando gestor edita manualmente:
- Manter validação de disponibilidade e setor
- Permitir violar regra de repetição (com aviso)
- Atualizar contador de alocações
- Alertar se criar desequilíbrio grande (diferença > 3)

## Melhorias Futuras

### Histórico de Faltas
Considerar histórico: colaboradores com muitas faltas têm prioridade reduzida.

### Agrupamento
Permitir agrupar colaboradores que preferem servir juntos.

### Restrições Personalizadas
Interface para gestor definir regras customizadas por igreja.

---
