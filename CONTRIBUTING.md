# FieldOps - Guia de Contribuição

Bem-vindo ao FieldOps! Para mantermos o código organizado, rastreável e padronizado entre todos os nossos repositórios (`backend`, `frontend`, `app` e `docs`), adotamos as convenções descritas abaixo.

## Padrão de Nomenclatura de Branches

Toda branch deve estar ligada a um Épico e usar o formato:
`<tipo>/<epico>-<slug-curto>`

**Exemplos:**
- `feat/EP-03-crud-clients`
- `fix/EP-08-outbox-retry`
- `chore/EP-01-ci-backend`
- `docs/EP-10-readme-execution`

## Mensagens de Commit

Adotamos [Conventional Commits](https://www.conventionalcommits.org/). As mensagens devem estar no modo imperativo e em inglês.

**Exemplos:**
- `feat(clients): add CRUD for clients with pagination`
- `fix(sync): prevent duplication in operation resend`

## Roteamento de Mudanças

Dependendo da mudança, direcione para o repositório correto:

| Mudança | Repositório |
| --- | --- |
| Regra de negócio, endpoint, migração de banco | `backend` |
| Tela, guard, serviço do painel administrativo | `frontend` |
| Tela, fluxo offline, recurso nativo mobile | `app` |
| Documentação geral, contrato compartilhado, ADR | `docs` |

## Fluxo de Pull Requests e Revisão

1. Crie sua branch a partir de `develop`.
2. Após finalizar, abra um Pull Request (PR) para a `develop`.
3. Utilize o **Template de Pull Request** padrão (carregado automaticamente) e preencha todos os campos. O template se baseia em nosso *Definition of Done (DoD)*.
4. **Política de Revisão:**
   - Todo PR deve ser revisado por pelo menos um outro membro da equipe.
   - O merge só pode ser feito após a aprovação da revisão e quando a integração contínua (CI) estiver passando com sucesso.
   - Itens do checklist do template de PR que não puderem ser concluídos devem ser explicitamente justificados e transformados em novas issues se necessário (pendências).
