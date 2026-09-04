<!--
Sync Impact Report
Versão: 1.0.0 → 1.1.0
Tipo de mudança: MINOR — expansão material de restrição existente; nenhum princípio
removido ou redefinido.

Princípios modificados: nenhum. Os cinco princípios permanecem inalterados.

Seções modificadas:
- Restrições de Produto e Tecnologia — o modelo de uso deixa de ser coletivo e passa a
  ser individual por membro, com seletor de membro, visão consolidada da família e
  ressalva explícita de que o seletor não é limite de segurança.

Seções adicionadas: nenhuma.
Seções removidas: nenhuma.

Itens diferidos:
- Stack tecnológica continua deliberadamente não fixada; pertence ao /speckit-plan.
-->

# Constituição do meubolsoapp

Aplicação web de controle de fluxo financeiro pessoal, usada pelos membros de uma
mesma família para registrar receitas e despesas.

## Core Principles

### I. Privacidade Primeiro (NÃO-NEGOCIÁVEL)

- É PROIBIDO armazenar dados bancários em qualquer hipótese: número de conta, agência,
  número de cartão, CVV, chave Pix ou credenciais de acesso a instituição financeira.
  Não há exceção, nem mediante consentimento.
- Todo processamento e armazenamento DEVE ocorrer no dispositivo do usuário por padrão.
- O envio de qualquer dado do usuário a serviço externo SÓ é permitido mediante
  consentimento explícito, que DEVE estar desligado por padrão e ser revogável a
  qualquer momento.
- Sem consentimento ativo, o aplicativo NÃO PODE emitir nenhuma requisição de rede
  contendo dado do usuário.
- Quando o consentimento está ativo, apenas o trecho de texto que o usuário acabou de
  digitar pode ser enviado. Histórico, saldo, lista de lançamentos e identificação de
  pessoas NUNCA podem ser incluídos.
- Recusar ou revogar o consentimento NUNCA bloqueia funcionalidade essencial.

Justificativa: o app lida com o gasto doméstico de uma família inteira. Limitar o que sai
do dispositivo a uma única frase digitada mantém a exposição verificável em revisão de
código, em vez de depender de confiança em serviço de terceiro.

### II. Funciona Offline, Sempre

- Toda funcionalidade essencial — registrar, editar, excluir, listar e consultar
  receitas e despesas, e ver o saldo — DEVE funcionar sem rede.
- Nenhuma funcionalidade essencial pode depender de IA.
- A interpretação de texto livre TEM como caminho padrão um parser local determinístico,
  executado no dispositivo. A IA é alternativa opcional, jamais pré-requisito.
- Dado do usuário NUNCA pode ser perdido silenciosamente. A falha de um recurso opcional
  DEVE ser visível na interface e oferecer o caminho manual equivalente.

Justificativa: um app de finanças que só funciona conectado é um app que falha justamente
no caixa do mercado. A IA é conveniência, não infraestrutura.

### III. Todo Requisito Tem Critério de Aceite

- Funcionalidade sem critério de aceite verificável NÃO entra no `spec.md`.
- O critério DEVE ser escrito em termos observáveis pelo usuário (dada uma entrada,
  qual o resultado), sem jargão de implementação.
- Uma tarefa só pode ser marcada como concluída após seu critério de aceite ser
  verificado na prática.

### IV. Escopo de MVP

- Nada que não possa ser testado e validado antes de o MVP estar pronto entra no escopo.
- Toda nova dependência, abstração ou camada exige justificativa escrita no `plan.md`.
  Sem justificativa, DEVE ser removida.
- Em caso de dúvida entre duas soluções, adota-se a mais simples.

Justificativa: este é um trabalho de faculdade. O objetivo é o básico bem feito e
demonstrável, não cobertura de funcionalidades.

### V. Português do Brasil como Padrão

- Toda a interface DEVE estar em português do Brasil.
- Valores monetários são exibidos no formato `R$ 1.234,56` e datas no formato
  `dd/mm/aaaa`.
- O parser de texto livre assume expressões de pt-BR, incluindo referências relativas
  de tempo ("ontem", "hoje") e formatos de valor ("250 reais", "R$ 250,00", "250,50").

## Restrições de Produto e Tecnologia

- Aplicação web com funcionamento offline-first no navegador.
- Uso familiar em dispositivo compartilhado. NÃO há autenticação no MVP.
- O controle é individual por membro da família, não coletivo. Cada lançamento DEVE
  identificar a qual membro pertence.
- A lista de membros DEVE ser gerenciável dentro do app: adicionar, renomear e remover.
- Ao abrir, o app DEVE perguntar qual membro está usando. A seleção NÃO é lembrada entre
  sessões.
- A visão padrão é filtrada pelo membro selecionado. O usuário PODE trocar de membro a
  qualquer momento e ver os lançamentos de outro, sem restrição.
- DEVE existir uma visão consolidada da família, somando todos os membros.
- O seletor de membro é um filtro de visualização, NÃO um limite de segurança nem de
  privacidade entre membros. Nenhuma funcionalidade pode assumir que ele protege dados.
- NÃO há backend próprio no MVP. A persistência é local ao navegador.
- Valores monetários DEVEM ser armazenados como inteiros em centavos. Ponto flutuante
  para dinheiro é proibido.
- Datas DEVEM ser armazenadas em ISO 8601 e formatadas para pt-BR apenas na apresentação.
- A stack tecnológica não é fixada por esta constituição; é decidida no `/speckit-plan`,
  desde que respeite as restrições acima.

## Fluxo de Desenvolvimento

- O desenvolvimento segue a ordem SDD: `constitution` → `specify` → `clarify` → `plan`
  → `tasks` → `implement`.
- O `spec.md` descreve o quê e o porquê, e NÃO PODE conter decisões de tecnologia.
- O `plan.md` DEVE verificar explicitamente a conformidade com esta constituição antes
  de a fase de tarefas começar.
- A entrada de dados por IA é a última funcionalidade do MVP a ser implementada, depois
  de a entrada manual e o parser local estarem funcionando e validados.

## Governance

- Esta constituição prevalece sobre qualquer outra prática, preferência ou sugestão de
  ferramenta durante o desenvolvimento deste projeto.
- Emendas exigem proposta escrita com justificativa, atualização da versão e da data de
  última alteração neste arquivo.
- O versionamento segue SemVer: MAJOR para remoção ou redefinição incompatível de
  princípio, MINOR para novo princípio ou expansão material, PATCH para esclarecimento
  de redação.
- A conformidade DEVE ser revisada a cada `plan.md` gerado. Violação sem justificativa
  escrita e aprovada bloqueia o avanço para a fase de tarefas.

**Version**: 1.1.0 | **Ratified**: 2026-09-04 | **Last Amended**: 2026-09-04
