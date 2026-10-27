# Coordenação entre agentes (Claude Code e ChatGPT/Codex)

Protocolo comum aos projetos da Julia Braga. Versão 1.0, de 26/10/2026.
O texto é igual em todos os repositórios. Decisões, estado, histórico e restrições
de cada produto ficam nos documentos do próprio projeto, indicados em
`docs/ESTADO-ATUAL.md`. Não copie preços, clientes, credenciais, permissões
ou regras de um produto para outro.

Este arquivo é orientação escrita. Não é uma trava técnica, não cria agentes, não
conecta um agente ao outro e não garante que uma ferramenta vá lê-lo. Os conflitos
precisam ser identificados antes da edição, e não depois.

## 1. Onde cada agente começa a leitura

| Ferramenta    | O que carrega sozinha                                                         | Garantia                                     |
| ------------- | ----------------------------------------------------------------------------- | -------------------------------------------- |
| Claude Code   | `CLAUDE.md` da raiz, que neste projeto importa o `AGENTS.md` com `@AGENTS.md` | Confira com `/context` no início da sessão   |
| ChatGPT/Codex | `AGENTS.md` da raiz e de subpastas                                            | Não lê `CLAUDE.md` nem segue importações `@` |

Sem um `CLAUDE.md`, as versões recentes do Claude Code leem o `AGENTS.md` sozinhas.
Quando existe um `CLAUDE.md`, ele passa a valer no lugar. Por isso a regra é ter uma
única fonte: as instruções do projeto ficam no `AGENTS.md`, e o `CLAUDE.md` apenas o
importa. Não mantenha cópias das mesmas regras nos dois arquivos.

A ordem de leitura antes de qualquer tarefa:

1. `AGENTS.md` (direto ou pela importação).
2. Este arquivo.
3. `docs/ESTADO-ATUAL.md`, que traz as frentes em andamento, o responsável, a branch e o próximo passo.
4. O registro de decisões indicado no `ESTADO-ATUAL.md`.

## 2. Antes de editar

Confira e registre:

- `git status`, branch atual, SHA local (`git rev-parse HEAD`) e SHA remoto (`git ls-remote --heads origin <branch>`);
- `git stash list`, arquivos novos não versionados e alterações não commitadas;
- PRs abertos e branches atualizadas recentemente por outro agente;
- a tabela de frentes do `ESTADO-ATUAL.md`.

Se outra frente estiver mexendo nos mesmos arquivos ou módulos, não edite. Registre o
conflito e pergunte à Julia quem segue.

## 3. Responsabilidade e isolamento

- Cada frente tem um único responsável (agente e ambiente), uma branch ou worktree própria e uma lista de módulos envolvidos.
- Frentes independentes usam branches ou worktrees separadas. Dois agentes não editam a mesma frente em paralelo.
- Um único responsável integra cada entrega.
- Registre na tabela de frentes: responsável, tarefa, branch/worktree, módulos, última atualização e próximo passo.

## 4. Preservação contínua

- Faça um checkpoint a cada bloco coerente de trabalho e sempre antes de parar ou trocar de agente. Não espere o limite de uso acabar.
- Trabalho parcial vai em commit marcado como `WIP:` na branch da frente, nunca misturado com trabalho pronto.
- Commit local não é cópia remota. Quando o envio estiver autorizado, confira antes os possíveis efeitos de CI e deploy e, depois do envio, confirme que o SHA remoto é igual ao local.
- Se não puder enviar, gere `git bundle create` (e confira com `git bundle verify`), patches e cópias dos arquivos novos relevantes, e guarde em destino autorizado e acessível. Um caminho temporário (`/tmp`, `/workspace`, pasta de sessão) não conta como cópia.
- Proibido: incluir segredos, tokens, senhas ou dados pessoais; force push; `reset --hard` ou qualquer descarte de alterações; apagar ou limpar stashes automaticamente.

## 5. Passagem de trabalho obrigatória

Ao parar ou trocar de agente, atualize o `ESTADO-ATUAL.md` (ou o documento de passagem que ele indicar) com:

- objetivo e decisões vigentes que se aplicam;
- responsável, ambiente e diretório;
- branch e SHAs local e remoto;
- arquivos alterados e trabalho parcial;
- testes executados, resultados, falhas e testes não executados;
- evidências e onde estão as cópias de recuperação;
- bloqueios, permissões pendentes e o próximo passo seguro;
- quando se aplicar, o estado separado de: código salvo, integração (merge), banco, funções, homologação e produção.

Não escreva "concluído" quando só existe código local, PR aberto ou preview.

## 6. Decisões e impactos

- Vale a decisão mais recente comprovadamente aprovada pela Julia, com data e origem (conversa, documento, PR).
- Decisões anteriores são marcadas como substituídas, sem apagar o histórico nem os aceites.
- Não invente decisões para preencher lacunas. Se faltar uma, registre como pendente.
- Antes de alterar, confira os impactos nos módulos relacionados, documentos, atendimento, segurança, pagamentos e dados, conforme o projeto.

## 7. Revisão e publicação

- O revisor independente analisa o SHA final. Mudanças posteriores exigem revalidação proporcional.
- Testes de outro commit não aprovam o atual. Sandbox não comprova pagamento real. CI verde não comprova publicação nem jornada completa.
- Respeite os requisitos de backup e recuperação do projeto antes de produção. Não altere proteções de branch, CI ou deploy para conseguir integrar.
- Merge e deploy exigem autorização específica da Julia.

## 8. Limites e autorizações

- Não compre créditos, não aumente orçamento e não contorne cotas, bloqueios ou negativas de acesso.
- Não reexecute um serviço bloqueado sem uma mudança comprovada de condição.
- Este protocolo não autoriza criar credenciais, ampliar permissões, publicar nem enviar mensagens a terceiros.
- Não configure comunicação automática entre agentes sem autorização específica.
- Não altere configurações globais das ferramentas sem avisar.
- As restrições especiais de cada projeto continuam valendo e ficam no `AGENTS.md` e no `ESTADO-ATUAL.md` dele. Não as transforme em regra presumida para outro projeto.
