# GPADS CodePath — Plano de sprints

Período: 03/11 a 01/12/2026. Equipe: Alane e Samara (frontend), Nathália (backend).

## Premissas e capacidade

- Disponibilidade confirmada: 20h por pessoa por semana, 60h de equipe.  
- Horas abaixo são estimativas de esforço, não horas já trabalhadas. Horas realizadas: a registrar desde o início, sem inventar apontamentos.  
- Por pessoa/semana: 5 dailys de 15min (1h15), planejamento de terça (1h) e review/retrospectiva de segunda (1h): 3h15. Revisão de código e correções já entram nas tarefas técnicas, sem dupla contagem.  
- Planejamento e review são adicionais à daily. Horários a combinar. Dailys de segunda a sexta: progresso desde ontem, próximo passo e impedimento. Atualizar o card e as horas realizadas após a daily.  
- Terças 03, 10, 17 e 24/11 iniciam as sprints. Segundas 09, 16, 23 e 30/11 ficam para ajustes, revisão de PRs e aceite. Não iniciar nova funcionalidade nas segundas.  
- Trabalho novo previsto de terça a sexta; fins de semana sem alocação obrigatória. Na semana de 20/11, concentrar implementação até quinta, 19/11; disponibilidade no feriado deve ser combinada, com daily assíncrona ou remarcada sem ampliar as 20h. Se a equipe tiver menos capacidade, replanejar o escopo.  
- 01/12 é marco de entrega, fora das quatro semanas: reservar proposta de 2h por pessoa para conferência (30min), apresentação (1h) e registro/encaminhamentos (30min), dentro das 20h da semana que começa nessa data.

## Review da etapa anterior

Confirmado pela usuária: Render/Vercel funcionam; login e sessão parcialmente validados; envio de relatórios e feedback já exercitados. Não recriar implantação nem feedback.

A validar: recuperação de acesso, cadastro e células, matriz de papéis, isolamento, estados reais e destino GitHub. Correção local de nome/data precisa ser conferida na versão integrada: a usuária ainda observa chave aleatória na publicação.

Os documentos anexos descrevem arquitetura e responsabilidades históricas, não comprovam conclusão de todos os itens. Alane mantém Services/Repositories/Auth/API; Samara mantém UI/layout/estados/responsividade. Nathália assume o backend nesta etapa, substituindo a divisão histórica Carlos/Jonathan. Papéis user/student e admin/avaliador exigem conciliação com a implementação atual.

## Objetivo do ciclo

Até 01/12, demonstrar usuário autenticado, sprint e tarefas Base44, relatório enviado, avaliação/feedback, publicação GitHub legível e acompanhamento com dados reais. Base44 nesta entrega é leitura de sprint e tarefas; escrita e sincronização bidirecional ficam fora do escopo.

## Calendário e carga

| Sprint | Período | Revisão | Alane técnica | Samara técnica | Nathália técnica | Cerimônias por pessoa | Reserva por pessoa |
| :---- | :---- | :---- | ----: | ----: | ----: | ----: | ----: |
| 1 | 03–09/11 | 09/11 | 14h | 14h | 14h | 3h15 | 2h45 |
| 2 | 10–16/11 | 16/11 | 16h | 16h | 16h | 3h15 | 0h45 |
| 3 | 17–23/11 | 23/11 | 16h | 16h | 16h | 3h15 | 0h45 |
| 4 | 24–30/11 | 30/11 | 12h | 12h | 12h | 3h15 | 4h45 |

Total das quatro sprints por pessoa: 58h técnicas \+ 13h de cerimônias \+ 9h de reserva \= 80h. Equipe: 174h técnicas \+ 39h de cerimônias \+ 27h de reserva \= 240h. A reserva cobre imprevistos, apoio e revisão adicional, não representa entrega já prometida. A sprint 3 tem pouca folga e depende da descoberta Base44.

## Regras para registrar na Base44

Um card por ID abaixo. Campos: título, sprint, responsável principal, início, prazo, estimativa\_horas, horas\_realizadas (vazio até apontamento), descrição, critérios de aceite e dependências. Todos os prazos são de 2026, no fuso America/Sao\_Paulo, ao fim do expediente combinado. Status inicial: Planejada, sem inferir que já iniciou ou concluiu.

Fluxo sugerido: Planejada, Em andamento, Em revisão, Em validação, Concluída ou Bloqueada. Registrar motivo, pessoa que pode desbloquear e próximo passo em cada bloqueio.

Dependências indicam contrato ou integração necessários. UI e fixtures podem começar com contrato preliminar durante a descoberta, mas a tarefa só conclui após validar a dependência. Entregas na mesma data exigem checkpoint intermediário na daily e integração antes do prazo. Cada tarefa tem um único responsável e horas incluem sua revisão/validação; as outras participantes usam suas próprias tarefas, evitando duplicar horas.

## Sprint 1 — Acesso e organização

### S1-N1 — Revisar papéis e contrato de acesso

- Responsável: Nathália.  
- Início: 03/11/2026. Prazo: 04/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: nenhuma tarefa prévia obrigatória.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Inventariar papéis reais e conciliar user/student, admin e avaliador/líder com o legado. Documentar matriz de ações por papel e célula e contratos de cadastro e perfil. Estudar o existente antes de alterar.

**Aceite:** Matriz aprovada pelas três, exemplos de respostas e diferenças do legado registradas.

### S1-N2 — Validar cadastro, células e isolamento

- Responsável: Nathália.  
- Início: 04/11/2026. Prazo: 06/11/2026.  
- Esforço estimado: 6h. Horas realizadas: a registrar.  
- Dependências: S1-N1.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Conferir catálogo real de células e vínculo no Firebase Auth/Firestore. Testar cadastro autorizado, dados inválidos, duplicidade, acesso entre estudantes e limites do avaliador por célula. Corrigir apenas falhas encontradas e registrar testes negativos.

**Aceite:** Usuário criado tem célula correta; chamadas diretas não permitem acesso indevido nem elevação de papel.

### S1-N3 — Revisar sessão e integrar a homologação de acesso

- Responsável: Nathália.  
- Início: 06/11/2026. Prazo: 09/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: S1-N2.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Revisar validação do token e respostas 401/403. Apoiar recuperação via Firebase Auth sem criar endpoint desnecessário. Executar regressão no ambiente já implantado, revisar PRs e atualizar contrato.

**Aceite:** Login/sessão preservados, erros coerentes e evidências integradas registradas.

### S1-A1 — Validar login, sessão e recuperação de senha

- Responsável: Alane.  
- Início: 03/11/2026. Prazo: 05/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: nenhuma tarefa prévia obrigatória.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Partir do login e sessão parcialmente validados. Revisar useAuth, renovação e expiração de token, logout e retorno de rota protegida. Implementar ou corrigir recuperação com Firebase Authentication e confirmar recebimento do e-mail e novo login.

**Aceite:** Conta de teste redefine senha e entra novamente; sessão expirada não fica presa em carregamento.

### S1-A2 — Integrar cadastro, células e regras de navegação

- Responsável: Alane.  
- Início: 05/11/2026. Prazo: 06/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S1-N1.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Conferir Services/Repositories, payloads e origem das opções de célula. Mapear papéis reais nas rotas e navegação conforme matriz. Tratar validações e erros da API sem regras de negócio no componente.

**Aceite:** Cadastro persiste os campos corretos e rotas exibem ações compatíveis com o papel.

### S1-A3 — Homologar acesso e revisar frontend

- Responsável: Alane.  
- Início: 06/11/2026. Prazo: 09/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: S1-A1, S1-A2, S1-N2.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Testar ponta a ponta com Samara e Nathália: login, reload, recuperação, cadastro e acesso negado. Revisar PRs, rodar build/lint e registrar resultados e falhas remanescentes.

**Aceite:** Fluxos críticos sem bloqueios, sem mocks no percurso homologado.

### S1-S1 — Revisar interface de acesso e recuperação

- Responsável: Samara.  
- Início: 03/11/2026. Prazo: 05/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: nenhuma tarefa prévia obrigatória.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Ajustar formulário de recuperação, confirmação de envio, retorno ao login e mensagens acessíveis. Reutilizar componentes e identidade gamificada existentes.

**Aceite:** Formulários funcionam por teclado e em celular; confirmação não expõe existência de conta.

### S1-S2 — Revisar cadastro e apresentação dos papéis

- Responsável: Samara.  
- Início: 05/11/2026. Prazo: 06/11/2026.  
- Esforço estimado: 6h. Horas realizadas: a registrar.  
- Dependências: S1-N1.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Conferir campos nome, e-mail, senha, tipo e célula. Exibir opções válidas do contrato, orientação de preenchimento, erros por campo e estados de envio. Revisar apresentação para admin/avaliador conforme decisão da matriz.

**Aceite:** Sem valores de célula inventados; erros legíveis; envio bloqueia cliques repetidos.

### S1-S3 — Revisar responsividade e homologar acesso

- Responsável: Samara.  
- Início: 06/11/2026. Prazo: 09/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: S1-S1, S1-S2, S1-A2.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Conferir telas de acesso e cadastro no desktop e celular, foco, contraste e feedback de permissões. Revisar PR de Alane e registrar evidências junto à equipe.

**Aceite:** Sem cortes ou sobreposição nos formulários e cenários de acesso aprovados.

## Sprint 2 — Relatórios, GitHub e ponte Base44

### S2-N1 — Padronizar estados do relatório

- Responsável: Nathália.  
- Início: 10/11/2026. Prazo: 11/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: S1-N1.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Revisar envio existente e definir estados e transições permitidas no backend, incluindo pedido de ajustes, reenvio e aprovação. Preservar feedback já implementado e histórico.

**Aceite:** Contrato descreve transições e servidor rejeita mudanças inválidas.

### S2-N2 — Corrigir nome, destino e repetição no GitHub

- Responsável: Nathália.  
- Início: 11/11/2026. Prazo: 13/11/2026.  
- Esforço estimado: 6h. Horas realizadas: a registrar.  
- Dependências: S2-N1.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Verificar se a correção local chegou à versão integrada. Gerar nome legível com nome normalizado e data de envio em America/Sao\_Paulo. Validar repositório/pasta configurados pelo líder, autorização e tratamento de colisões e novas tentativas. Manter ID interno separado do nome do arquivo.

**Aceite:** Publicação real de teste em pasta correta com nome relatorio\_quinzenal\_nathalia\_12\_11\_2026.md ou equivalente à data real, sem duplicação acidental.

### S2-N3 — Estudar acesso e contrato da Base44

- Responsável: Nathália.  
- Início: 10/11/2026. Prazo: 13/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: S1-N1.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Com acesso autorizado ao app utilizado pela equipe, identificar entidades reais de sprint e tarefa, autenticação disponível, limites e permissões. Mapear UID do CodePath ao usuário Base44 e regra de sprint atual. Propor consulta somente leitura via backend, sem assumir endpoints ou nomes de entidades.

**Aceite:** Prova de leitura autorizada ou bloqueio documentado; contrato com campos, vínculo de usuário e cenários sem sprint.

### S2-N4 — Revisar integração de relatórios e ponte Base44

- Responsável: Nathália.  
- Início: 16/11/2026. Prazo: 16/11/2026.  
- Esforço estimado: 2h. Horas realizadas: a registrar.  
- Dependências: S2-N2, S2-N3.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Revisar PRs e executar cenários de envio, avaliação existente, publicação e falha do GitHub. Consolidar contrato Base44 com Alane e Samara e separar pendências externas.

**Aceite:** Review registra evidências do GitHub e prontidão ou bloqueio da Base44.

### S2-A1 — Melhorar envio e integrar estados

- Responsável: Alane.  
- Início: 10/11/2026. Prazo: 12/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S2-N1.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Revisar Services, serialização do período, data e referência ao código. Preservar texto após erro, impedir envio duplicado e consumir estados reais. Revalidar feedback já existente e reenvio sem reconstruir a funcionalidade.

**Aceite:** Relatório mantém dados após erro e estado acompanha resposta persistida.

### S2-A2 — Integrar resultado da publicação GitHub

- Responsável: Alane.  
- Início: 12/11/2026. Prazo: 13/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: S2-N2.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Exibir nome e link devolvidos pelo backend, distinguir aprovação de publicação e tratar falha recuperável sem sucesso falso. Validar destino quando permitido ao líder.

**Aceite:** Interface reflete arquivo e link reais e nova tentativa não duplica publicação.

### S2-A3 — Preparar Repository da Base44 no frontend

- Responsável: Alane.  
- Início: 10/11/2026. Prazo: 13/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S2-N3.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Definir modelo de sprint/tarefa e Repository para consumir futura API do CodePath. Preparar fixtures contratuais de usuário com sprint, sem sprint e serviço indisponível. Não colocar chave Base44 no navegador nem ligar mocks à produção.

**Aceite:** Camada preparada com contrato revisado e fixtures identificadas para desenvolvimento.

### S2-A4 — Revisar fluxo de relatórios e contrato Base44

- Responsável: Alane.  
- Início: 16/11/2026. Prazo: 16/11/2026.  
- Esforço estimado: 2h. Horas realizadas: a registrar.  
- Dependências: S2-A1, S2-A2, S2-A3.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Executar fluxo integrado com a equipe, revisar frontend e documentar mapeamento dos campos e pendências da integração.

**Aceite:** Build/lint e percurso crítico aprovados ou defeitos registrados com responsável.

### S2-S1 — Ajustar formulário e estados de avaliação

- Responsável: Samara.  
- Início: 10/11/2026. Prazo: 12/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S2-N1.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Revisar hierarquia, campos e confirmação do envio. Exibir período, data, código e estados reais com rótulos compreensíveis. Manter feedback existente visível e ação de reenvio quando autorizada.

**Aceite:** Estudante entende situação e próxima ação sem depender apenas de cor.

### S2-S2 — Corrigir interface da publicação GitHub

- Responsável: Samara.  
- Início: 12/11/2026. Prazo: 13/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: S2-N2.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Mostrar título e data legíveis, pasta/repositório quando aplicável, link do arquivo e erro com opção de nova tentativa. Revisar painel do avaliador em celular.

**Aceite:** ID aleatório não aparece como título do arquivo; sucesso só após confirmação da API.

### S2-S3 — Preparar visual de sprint e tarefas Base44

- Responsável: Samara.  
- Início: 10/11/2026. Prazo: 13/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S2-N3.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Desenhar nos componentes existentes a área da sprint atual com período, status e lista de tarefas atribuídas ao usuário. Prever sem sprint, sem tarefas e indisponibilidade com última atualização. Implementar com fixtures do contrato.

**Aceite:** Layout revisado com Alane e preparado para dados reais sem prometer sincronização bidirecional.

### S2-S4 — Homologar visual e revisar ponte Base44

- Responsável: Samara.  
- Início: 16/11/2026. Prazo: 16/11/2026.  
- Esforço estimado: 2h. Horas realizadas: a registrar.  
- Dependências: S2-S1, S2-S2, S2-S3.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Revisar PRs, conferir envio e publicação no desktop/celular e alinhar estados visuais da Base44.

**Aceite:** Evidências visuais e ajustes de segunda registrados.

## Sprint 3 — Acompanhamento e Base44 real

### S3-N1 — Implementar leitura Base44 pelo backend

- Responsável: Nathália.  
- Início: 17/11/2026. Prazo: 19/11/2026.  
- Esforço estimado: 6h. Horas realizadas: a registrar.  
- Dependências: S2-N3, S2-N4.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Implementar adaptador conforme descoberta, credenciais no servidor e filtro pelo vínculo do usuário autenticado. Retornar sprint atual, tarefas atribuídas, status, prazo e atualização. Tratar timeout, paginação se necessária e ausência de vínculo.

**Aceite:** Dois usuários veem apenas suas tarefas autorizadas e falhas externas têm resposta controlada.

### S3-N2 — Consolidar pendências, histórico e filtros

- Responsável: Nathália.  
- Início: 17/11/2026. Prazo: 19/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S2-N1, S1-N2.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Revisar consultas de entregues, pendentes e aguardando avaliação. Definir denominadores e período, consultar histórico por estudante e filtrar célula, período e situação respeitando escopo.

**Aceite:** Totais batem com registros e filtros não expõem outra célula indevidamente.

### S3-N3 — Revisar IA e homologar integrações

- Responsável: Nathália.  
- Início: 19/11/2026. Prazo: 23/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S3-N1, S3-N2.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Validar limite diário atual de 2 consultas por usuário no servidor, fuso de renovação e concorrência. Simular falhas e limite sem consumir cotas reais automaticamente. Na segunda revisar PRs e homologar Base44, indicadores e IA.

**Aceite:** Limite independe do navegador e erros preservam o rascunho editável.

### S3-A1 — Conectar sprint e tarefas reais da Base44

- Responsável: Alane.  
- Início: 17/11/2026. Prazo: 19/11/2026.  
- Esforço estimado: 6h. Horas realizadas: a registrar.  
- Dependências: S3-N1, S2-A3, S2-S3.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Substituir fixtures pelo Repository da API CodePath. Ligar dados ao usuário autenticado, tratar ausência de vínculo e atualização sem misturar tarefas de usuários.

**Aceite:** Conta vinculada vê sprint e tarefas reais sem segredo Base44 no navegador.

### S3-A2 — Integrar indicadores, histórico e filtros

- Responsável: Alane.  
- Início: 17/11/2026. Prazo: 19/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S3-N2.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Consumir dados consolidados e ligar filtros à API. Manter estado ao navegar e ignorar respostas antigas após troca rápida de filtro. Diferenciar resultado vazio de erro.

**Aceite:** Indicadores consistentes, histórico consultável e filtros corretos.

### S3-A3 — Revisar fluxo IA e integração final

- Responsável: Alane.  
- Início: 19/11/2026. Prazo: 23/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S3-N3, S3-A1, S3-A2.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Conectar contador/limite do servidor e preservar rascunho editável antes do envio. Revisar frontend e homologar Base44, dashboard e relatórios na segunda.

**Aceite:** Usuário revisa texto antes de enviar e compreende limite/erro.

### S3-S1 — Finalizar sprint e tarefas na interface

- Responsável: Samara.  
- Início: 17/11/2026. Prazo: 19/11/2026.  
- Esforço estimado: 6h. Horas realizadas: a registrar.  
- Dependências: S2-S3, S3-A1.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Ligar componentes ao fluxo de Alane. Mostrar sprint, período, título, prazo e situação das tarefas. Revisar listas longas, vazio e indisponibilidade.

**Aceite:** Usuário identifica sua sprint e tarefas sem interpretar IDs técnicos.

### S3-S2 — Destacar pendências e histórico com filtros

- Responsável: Samara.  
- Início: 17/11/2026. Prazo: 19/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S3-N2, S3-A2.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Ajustar dashboard para localizar pendências rapidamente. Revisar histórico por estudante e controles de célula, período e situação com contagem e estado ativo acessíveis.

**Aceite:** Líder localiza estudante pendente e consulta seu histórico.

### S3-S3 — Revisar acessibilidade, estados e rascunho IA

- Responsável: Samara.  
- Início: 19/11/2026. Prazo: 23/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S3-S1, S3-S2, S3-A3.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Conferir foco, teclado, contraste e responsividade. Mostrar loading, vazio, erro e limite IA com ação de recuperação. Revisar PRs e homologar na segunda.

**Aceite:** Sem cortes no celular ou informação só por cor, mensagens orientam próxima ação.

## Sprint 4 — Homologação e entrega

### S4-A1 — Homologar e corrigir falhas da área

- Responsável: Alane.  
- Início: 24/11/2026. Prazo: 25/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S3-A3.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Executar fluxos integrados de acesso, envio, avaliação, GitHub, Base44 e IA. Corrigir falhas da camada de dados frontend e conferir ausência de mocks.

**Aceite:** Cenários críticos com evidências e sem defeito bloqueador pendente.

### S4-A2 — Documentar área e preparar demonstração

- Responsável: Alane.  
- Início: 26/11/2026. Prazo: 27/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: S4-A1.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Atualizar instalação, variáveis públicas, build e diagnóstico. Preparar contas e roteiro repetível com dados coerentes.

**Aceite:** Outra integrante consegue seguir o guia e repetir seu percurso.

### S4-A3 — Revisar versão final e ensaiar apresentação

- Responsável: Alane.  
- Início: 30/11/2026. Prazo: 30/11/2026.  
- Esforço estimado: 3h. Horas realizadas: a registrar.  
- Dependências: S4-A2.  
- Apoio: Nathália no contrato e API; Samara nos componentes. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Revisar PRs e checklist da área, executar ensaio conjunto, conferir links e registrar versão candidata e pendências residuais. Segunda exclusiva para ajustes e revisão.

**Aceite:** Versão candidata identificada, demonstração repetível e bloqueadores encerrados.

### S4-S1 — Homologar e corrigir falhas da área

- Responsável: Samara.  
- Início: 24/11/2026. Prazo: 25/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S3-S3.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Testar com pelo menos um estudante e um líder. Registrar dificuldades e corrigir telas, mensagens e responsividade prioritárias.

**Aceite:** Cenários críticos com evidências e sem defeito bloqueador pendente.

### S4-S2 — Documentar área e preparar demonstração

- Responsável: Samara.  
- Início: 26/11/2026. Prazo: 27/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: S4-S1.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Produzir guia curto de estudante e líder com telas atuais. Preparar roteiro de navegação e conferir consistência visual.

**Aceite:** Outra integrante consegue seguir o guia e repetir seu percurso.

### S4-S3 — Revisar versão final e ensaiar apresentação

- Responsável: Samara.  
- Início: 30/11/2026. Prazo: 30/11/2026.  
- Esforço estimado: 3h. Horas realizadas: a registrar.  
- Dependências: S4-S2.  
- Apoio: Alane na ligação dos dados; Nathália nos contratos. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Revisar PRs e checklist da área, executar ensaio conjunto, conferir links e registrar versão candidata e pendências residuais. Segunda exclusiva para ajustes e revisão.

**Aceite:** Versão candidata identificada, demonstração repetível e bloqueadores encerrados.

### S4-N1 — Homologar e corrigir falhas da área

- Responsável: Nathália.  
- Início: 24/11/2026. Prazo: 25/11/2026.  
- Esforço estimado: 5h. Horas realizadas: a registrar.  
- Dependências: S3-N3.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Executar autorização, persistência, GitHub, Base44 e falhas externas. Corrigir bloqueadores da API e registrar testes significativos.

**Aceite:** Cenários críticos com evidências e sem defeito bloqueador pendente.

### S4-N2 — Documentar área e preparar demonstração

- Responsável: Nathália.  
- Início: 26/11/2026. Prazo: 27/11/2026.  
- Esforço estimado: 4h. Horas realizadas: a registrar.  
- Dependências: S4-N1.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Atualizar README, contratos, variáveis sem segredos, execução, deploy existente e retorno à versão estável. Preparar dados autorizados.

**Aceite:** Outra integrante consegue seguir o guia e repetir seu percurso.

### S4-N3 — Revisar versão final e ensaiar apresentação

- Responsável: Nathália.  
- Início: 30/11/2026. Prazo: 30/11/2026.  
- Esforço estimado: 3h. Horas realizadas: a registrar.  
- Dependências: S4-N2.  
- Apoio: Alane no contrato/integração; Samara nos estados da interface. Apoio usa a capacidade dos respectivos cards.

**Descrição:** Revisar PRs e checklist da área, executar ensaio conjunto, conferir links e registrar versão candidata e pendências residuais. Segunda exclusiva para ajustes e revisão.

**Aceite:** Versão candidata identificada, demonstração repetível e bloqueadores encerrados.

## Relações e caminho crítico

| Entrega | Cadeia principal |
| :---- | :---- |
| Acesso seguro | S1-N1, S1-N2, S1-A2 e S1-S2, depois homologação S1-A3/S1-S3/S1-N3 |
| Recuperação | S1-A1 e S1-S1, depois homologação da sprint 1 |
| Relatório e GitHub | S2-N1, S2-A1/S2-S1, S2-N2, S2-A2/S2-S2, depois revisão S2-\*-4 |
| Base44 | S2-N3, S2-A3/S2-S3, S2-N4, S3-N1, S3-A1, S3-S1, depois homologação |
| Indicadores e histórico | S3-N2, S3-A2, S3-S2, depois homologação |
| IA | S3-N3, S3-A3, S3-S3 com checkpoint antecipado do contrato |
| Entrega | Homologação S4-*\-1, documentação S4-*\-2, ensaio S4-\*-3, marco 01/12 |

## Critérios de aceite por sprint

1. Conta nova entra com célula correta; recuperação funciona; acesso cruzado indevido falha no servidor.  
2. Envio, estado, feedback existente e reenvio funcionam. GitHub recebe arquivo com título/data na pasta autorizada. Base44 tem contrato e acesso estudados.  
3. Usuário vinculado vê sua sprint e tarefas reais. Líder identifica pendências e histórico com filtros. IA preserva rascunho e respeita limite no servidor.  
4. Fluxo completo homologado, guias atualizados, versão candidata identificada e ensaio sem bloqueadores.

## Base44: descoberta, acesso e limite de escopo

Até 10/11, a coordenação deve disponibilizar acesso autorizado ao app Base44, contato com quem administra o app e contas de teste. Não enviar chaves em cards ou no chat. Até 13/11, Nathália registra como consultar os dados e vincular identidades. Ainda não conhecemos o schema específico do app da equipe, então nenhum endpoint externo é presumido neste plano.

O backend CodePath concentra autorização e integração. A definição de sprint atual deve considerar vínculo do usuário, participação/célula e datas/status conforme o app real, com regra explícita para múltiplas sprints. Tarefas são as atribuídas ao usuário na sprint escolhida; acesso gerencial depende da matriz de permissões. Não assumir que Firebase ID Token autentica diretamente na Base44.

Se o acesso ou recurso necessário não estiver disponível até 13/11, registrar bloqueio e decidir na revisão de 16/11 como liberar acesso ou reduzir outra entrega para acomodar o trabalho. Fixtures servem para avançar UI, mas não contam como integração real. Priorizar Base44 e fluxo de relatórios; se houver excesso de esforço, reduzir refinamentos visuais e expansão de filtros antes de comprometer autorização ou inventar conclusão.

Referência oficial consultada para orientar a descoberta: [Base44 para desenvolvedores](https://base44.com/developers). O documento não define os campos do app da equipe.

## Definição de concluído e rotina de revisão

- PR vinculado ao card e revisão por outra integrante. Alane/Samara revisam frontend mutuamente; contrato do backend recebe leitura de Alane, com testes/evidências de Nathália e walkthrough quando a revisão exigir conhecimento específico.  
- Critério de aceite demonstrado, dados persistidos e permissões verificadas no backend.  
- Testes significativos para regra de negócio/integração, build/lint para frontend e regressão das áreas afetadas.  
- Erros, carregamento e vazio tratados quando aplicáveis, sem mocks disfarçados de integração real.  
- Horas realizadas e evidências anexadas ao card, documentação atualizada quando o contrato muda.

## Marco de 01/12/2026

| ID | Responsável | Prazo | Estimativa | Descrição e aceite | Dependências |
| :---- | :---- | :---- | ----: | :---- | :---- |
| FINAL-A | Alane | 01/12/2026 | 2h | Conferir ambiente, apresentar sua área, acompanhar demonstração conjunta e registrar encaminhamentos. Aceite: fluxo apresentado e versão entregue identificada. Horas realizadas a registrar. | S4-A3 |
| FINAL-S | Samara | 01/12/2026 | 2h | Conferir ambiente, apresentar sua área, acompanhar demonstração conjunta e registrar encaminhamentos. Aceite: fluxo apresentado e versão entregue identificada. Horas realizadas a registrar. | S4-S3 |
| FINAL-N | Nathália | 01/12/2026 | 2h | Conferir ambiente, apresentar sua área, acompanhar demonstração conjunta e registrar encaminhamentos. Aceite: fluxo apresentado e versão entregue identificada. Horas realizadas a registrar. | S4-N3 |

## Fora do escopo deste mês

Novas mecânicas de pontuação, expansão de ranking, mais avatares, reformulação total do layout e sincronização bidirecional Base44. Feedback e deploy existentes recebem regressão e correção quando necessária. Não refazer funcionalidades prontas.