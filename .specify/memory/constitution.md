<!--
Sync Impact Report
Versão: 1.1.0 → 2.0.0
Tipo de mudança: MAJOR — duas redefinições incompatíveis: o modelo de uso deixa de ser
por membro da família e passa a ser estritamente individual; a entrada de dados deixa de
ter o parser de texto livre como caminho padrão e passa a ser por formulário estruturado.
O parser local determinístico é removido do escopo.

Princípios modificados:
- I. Privacidade Primeiro — justificativa deixa de citar a família; cláusulas de
  consentimento mantidas (aplicam-se à entrada opcional por IA).
- II. Funciona Offline, Sempre — caminho padrão de entrada passa de "parser local
  determinístico" para "formulário estruturado"; parser removido.
- V. Português do Brasil como Padrão — removida a cláusula sobre expressões pt-BR do
  parser; mantidos interface e formatos de exibição.

Seções modificadas:
- Restrições de Produto e Tecnologia — removido todo o modelo de membros (seletor,
  lista gerenciável, visão consolidada). Adicionadas regras de uso individual sem
  identificação e de entrada por controles com formato controlado.
- Fluxo de Desenvolvimento — a entrada por IA passa a ser implementada após o formulário
  estruturado (antes: após entrada manual e parser local).

Seções adicionadas: nenhuma.
Seções removidas: nenhuma (apenas conteúdo dentro de seções existentes).

Itens diferidos:
- Stack tecnológica continua deliberadamente não fixada; pertence ao /speckit-plan.
-->

# Constituição do meubolsoapp

Aplicação web de controle de fluxo financeiro pessoal, de uso individual, para registrar
receitas e despesas.

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
  digitar pode ser enviado. Histórico, saldo e lista de lançamentos NUNCA podem ser
  incluídos.
- Recusar ou revogar o consentimento NUNCA bloqueia funcionalidade essencial.

Justificativa: o app lida com o gasto pessoal do usuário. Limitar o que sai do dispositivo
a uma única frase digitada mantém a exposição verificável em revisão de código, em vez de
depender de confiança em serviço de terceiro.

### II. Funciona Offline, Sempre

- Toda funcionalidade essencial — registrar, editar, excluir, listar e consultar
  receitas e despesas, e ver o saldo — DEVE funcionar sem rede.
- Nenhuma funcionalidade essencial pode depender de IA.
- O caminho padrão de entrada de dados é o formulário estruturado, executado inteiramente
  no dispositivo. A IA é alternativa opcional, jamais pré-requisito.
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

## Restrições de Produto e Tecnologia

- Aplicação web com funcionamento offline-first no navegador.
- Uso estritamente individual. O app assume que quem o abre está no próprio navegador.
  NÃO há autenticação, identificação de usuário, perfis, membros ou qualquer
  compartilhamento de dados entre pessoas no MVP.
- A entrada de dados é por formulário estruturado: o usuário seleciona opções (botões,
  listas) e digita apenas em campos com formato controlado (valor, data, descrição
  curta). NÃO existe campo de texto livre interpretado pelo app.
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
  de o formulário estruturado estar funcionando e validado.

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

**Version**: 2.0.0 | **Ratified**: 2026-09-04 | **Last Amended**: 2026-09-10
