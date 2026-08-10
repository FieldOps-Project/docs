# 06 - Casos de Uso

> Fonte: [Projeto FieldOps no Notion](https://glaucotodesco.notion.site/Projeto-FieldOps-3af5d52a81f580c0b92ec261c686abb4)
>
> Página Notion: 3af5d52a-81f5-81ba-9ab9-f2ee5c9ee7d4

# 6. Casos de Uso

## 6.1 Catálogo resumido

| ID | Caso de uso | Ator principal | Canal | Prioridade |
| --- | --- | --- | --- | --- |
| UC-01 | Autenticar usuário | Todos | Mobile/Web | P0 |
| UC-02 | Gerenciar usuários | Administrador | Web | P0 |
| UC-03 | Gerenciar clientes, locais e equipamentos | Administrador/Supervisor | Web | P0 |
| UC-04 | Criar modelo de inspeção | Supervisor | Web | P0 |
| UC-05 | Publicar versão do modelo | Supervisor | Web | P0 |
| UC-06 | Agendar e atribuir inspeção | Supervisor | Web | P0 |
| UC-07 | Baixar inspeções atribuídas | Técnico | Mobile | P0 |
| UC-08 | Identificar equipamento por QR Code | Técnico | Mobile | P0 |
| UC-09 | Iniciar inspeção | Técnico | Mobile | P0 |
| UC-10 | Responder checklist | Técnico | Mobile | P0 |
| UC-11 | Registrar evidência | Técnico | Mobile | P0 |
| UC-12 | Registrar não conformidade | Técnico | Mobile | P0 |
| UC-13 | Concluir inspeção offline | Técnico | Mobile | P0 |
| UC-14 | Sincronizar alterações | Técnico/Sistema | Mobile/API | P0 |
| UC-15 | Acompanhar inspeções | Supervisor | Web | P0 |
| UC-16 | Revisar inspeção | Supervisor | Web | P0 |
| UC-17 | Aprovar ou reprovar inspeção | Supervisor | Web | P0 |
| UC-18 | Consultar histórico | Administrador/Supervisor | Web | P1 |
| UC-19 | Consultar indicadores | Supervisor | Web | P1 |
| UC-20 | Consultar resultado como cliente | Cliente | Web | P2 |

## 6.2 UC-01 — Autenticar usuário

**Ator principal:** qualquer usuário ativo.

**Pré-condições:** usuário cadastrado e ativo.

**Gatilho:** o usuário informa suas credenciais.

### Fluxo principal

1. O usuário informa e-mail e senha.

1. A aplicação valida o formato dos campos.

1. A aplicação envia as credenciais à API.

1. A API valida o usuário e a senha.

1. A API retorna tokens e informações mínimas do perfil.

1. A aplicação armazena a sessão de forma apropriada ao canal.

1. O usuário é direcionado à área autorizada.

### Fluxos alternativos

- Credenciais inválidas: exibir mensagem sem informar qual campo está incorreto.

- Usuário inativo: negar acesso e orientar contato com a administração.

- Falha de rede no mobile: permitir somente o acesso offline quando existir uma sessão previamente válida e dados locais autorizados.

- Token expirado: tentar renovação conforme contrato da API.

### Pós-condições

- Sessão autenticada criada; ou

- acesso negado sem alteração indevida de dados.

## 6.3 UC-04 — Criar modelo de inspeção

**Ator principal:** supervisor.

**Pré-condições:** usuário autenticado com permissão.

**Gatilho:** necessidade de padronizar um tipo de inspeção.

### Fluxo principal

1. O supervisor cria um modelo em estado de rascunho.

1. Informa título, descrição e categoria.

1. Cria uma ou mais seções.

1. Adiciona itens às seções.

1. Define o tipo de resposta de cada item.

1. Define obrigatoriedade, regras de evidência e ordem.

1. Salva o rascunho.

1. Valida a prévia do checklist.

1. Publica uma versão do modelo.

### Fluxos alternativos

- Modelo sem itens: publicação bloqueada.

- Item sem tipo de resposta: publicação bloqueada.

- Modelo já utilizado: alterações estruturais geram nova versão.

### Pós-condições

- Uma versão imutável do modelo fica disponível para novas inspeções.

## 6.4 UC-06 — Agendar e atribuir inspeção

**Ator principal:** supervisor.

**Pré-condições:** cliente, local, equipamento, técnico e modelo válidos.

**Gatilho:** necessidade de executar uma inspeção.

### Fluxo principal

1. O supervisor seleciona o modelo publicado.

1. Seleciona cliente, local e equipamento, quando aplicável.

1. Define técnico, prioridade e data prevista.

1. Informa orientações adicionais.

1. Confirma o agendamento.

1. A API cria a inspeção e seu snapshot de itens.

1. A inspeção fica disponível para sincronização pelo técnico.

### Fluxos alternativos

- Técnico inativo: operação bloqueada.

- Modelo sem versão publicada: operação bloqueada.

- Equipamento incompatível com o local: operação bloqueada.

### Pós-condições

- Inspeção criada em estado atribuído.

## 6.5 UC-07 — Baixar inspeções atribuídas

**Ator principal:** técnico.

**Pré-condições:** sessão válida e conectividade.

**Gatilho:** atualização manual ou automática dos dados.

### Fluxo principal

1. O aplicativo consulta alterações disponíveis para o técnico.

1. A API retorna inspeções e dados auxiliares autorizados.

1. O aplicativo grava os dados em SQLite.

1. O aplicativo atualiza a data da última sincronização.

1. As inspeções ficam disponíveis offline.

### Fluxos alternativos

- Falha parcial: manter dados anteriores e registrar o erro.

- Inspeção cancelada no servidor: atualizar o estado local sem apagar respostas pendentes de forma silenciosa.

### Pós-condições

- Base local atualizada ou preservada em caso de falha.

## 6.6 UC-08 — Identificar equipamento por QR Code

**Ator principal:** técnico.

**Pré-condições:** permissão de câmera concedida e equipamento com código cadastrado.

**Gatilho:** técnico inicia a leitura.

### Fluxo principal

1. O aplicativo solicita ou verifica a permissão da câmera.

1. O técnico aponta a câmera para o QR Code.

1. O aplicativo lê o identificador.

1. O aplicativo procura o equipamento na base local.

1. Quando necessário e possível, consulta a API.

1. Exibe os dados do equipamento.

1. O técnico confirma o vínculo com a inspeção.

### Fluxos alternativos

- Código desconhecido: informar que o equipamento não foi localizado.

- Equipamento diferente do previsto: solicitar confirmação ou bloquear conforme regra da inspeção.

- Permissão negada: disponibilizar identificação manual quando permitido.

## 6.7 UC-09 — Iniciar inspeção

**Ator principal:** técnico.

**Pré-condições:** inspeção atribuída ao técnico e disponível localmente.

**Gatilho:** seleção da ação “Iniciar inspeção”.

### Fluxo principal

1. O aplicativo apresenta os dados principais.

1. O técnico confirma o início.

1. O aplicativo registra data e hora do dispositivo.

1. Solicita a localização, quando prevista.

1. Altera o estado local para em andamento.

1. Registra a operação na outbox.

1. Apresenta o checklist.

### Fluxos alternativos

- Inspeção cancelada: início bloqueado após atualização do estado.

- Localização negada: continuar somente se a política da inspeção permitir.

## 6.8 UC-10 — Responder checklist

**Ator principal:** técnico.

**Pré-condições:** inspeção em andamento.

**Gatilho:** abertura de uma seção ou item.

### Fluxo principal

1. O aplicativo apresenta as seções e os itens do snapshot.

1. Renderiza o componente adequado ao tipo de resposta.

1. O técnico informa a resposta.

1. O aplicativo valida o valor.

1. A resposta é salva imediatamente no banco local.

1. O progresso é atualizado.

1. A operação pendente é registrada para sincronização.

### Fluxos alternativos

- Resposta não conforme: exigir observação quando configurado.

- Item crítico: exigir evidência quando aplicável.

- Valor inválido: impedir o avanço e apresentar orientação.

## 6.9 UC-11 — Registrar evidência

**Ator principal:** técnico.

**Pré-condições:** permissão de câmera ou arquivos e inspeção editável.

**Gatilho:** ação de adicionar evidência.

### Fluxo principal

1. O técnico escolhe capturar foto ou selecionar imagem.

1. O aplicativo obtém a imagem.

1. Exibe uma prévia.

1. O técnico confirma ou refaz a captura.

1. O aplicativo associa a evidência à inspeção, resposta ou não conformidade.

1. O arquivo é armazenado localmente.

1. A evidência é adicionada à fila de sincronização.

### Fluxos alternativos

- Arquivo excede o limite: informar e permitir nova captura.

- Falha no envio: manter o arquivo local e marcar como pendente.

- Exclusão antes da sincronização: remover localmente e cancelar a operação pendente.

## 6.10 UC-12 — Registrar não conformidade

**Ator principal:** técnico.

**Pré-condições:** inspeção em andamento.

**Gatilho:** identificação de problema ou resposta configurada como não conforme.

### Fluxo principal

1. O técnico seleciona ou confirma a criação da não conformidade.

1. Informa título, descrição e criticidade.

1. Adiciona evidências quando necessário.

1. O aplicativo valida os campos.

1. Salva o registro localmente.

1. Relaciona a não conformidade ao item e à inspeção.

1. Adiciona a operação à outbox.

## 6.11 UC-13 — Concluir inspeção offline

**Ator principal:** técnico.

**Pré-condições:** inspeção em andamento e dados locais disponíveis.

**Gatilho:** ação de concluir.

### Fluxo principal

1. O aplicativo valida os itens obrigatórios.

1. Valida observações e evidências exigidas.

1. Apresenta o resumo.

1. O técnico confirma a conclusão.

1. O aplicativo registra data e hora local.

1. O estado local muda para aguardando sincronização.

1. As respostas ficam bloqueadas para edição comum.

1. A operação de conclusão é adicionada à outbox.

### Fluxos alternativos

- Itens obrigatórios pendentes: conclusão bloqueada e itens destacados.

- Evidência obrigatória ausente: conclusão bloqueada.

## 6.12 UC-14 — Sincronizar alterações

**Ator principal:** sistema, com acompanhamento do técnico.

**Pré-condições:** sessão válida, conectividade e operações pendentes.

**Gatilho:** ação manual, abertura do aplicativo ou evento de conectividade.

### Fluxo principal

1. O aplicativo identifica operações pendentes.

1. Organiza as operações por dependência.

1. Envia um lote com identificadores idempotentes.

1. A API valida autorização e versão dos registros.

1. A API processa cada operação.

1. Retorna o resultado individual de cada item.

1. O aplicativo marca operações concluídas.

1. Mantém falhas como pendentes e registra a mensagem.

1. Baixa alterações do servidor.

1. Atualiza o estado da inspeção.

### Fluxos alternativos

- Token expirado: renovar e repetir de maneira controlada.

- Conflito: preservar a alteração local, informar o usuário e aplicar a política definida.

- Falha de arquivo: sincronizar dados textuais e manter a evidência pendente, quando permitido.

- Reenvio: a API retorna o resultado anterior sem duplicar dados.

## 6.13 UC-16 — Revisar inspeção

**Ator principal:** supervisor.

**Pré-condições:** inspeção enviada e disponível para revisão.

**Gatilho:** abertura da inspeção na interface administrativa.

### Fluxo principal

1. O supervisor visualiza o resumo.

1. Consulta as informações do equipamento e do técnico.

1. Navega pelas seções do checklist.

1. Analisa respostas, observações, localização e evidências.

1. Consulta não conformidades.

1. Registra comentários de revisão quando necessário.

1. Decide aprovar ou reprovar.

## 6.14 UC-17 — Aprovar ou reprovar inspeção

**Ator principal:** supervisor.

**Pré-condições:** inspeção em revisão.

**Gatilho:** decisão do supervisor.

### Fluxo de aprovação

1. O supervisor seleciona aprovar.

1. Confirma a decisão.

1. A API registra usuário, data e comentário.

1. A inspeção passa para aprovada.

1. O conteúdo fica protegido contra alterações comuns.

### Fluxo de reprovação

1. O supervisor seleciona reprovar.

1. Informa obrigatoriamente o motivo.

1. Pode indicar itens que exigem correção.

1. A API registra a revisão.

1. A inspeção passa para reprovada.

1. O técnico poderá recebê-la novamente conforme fluxo de correção.

---

