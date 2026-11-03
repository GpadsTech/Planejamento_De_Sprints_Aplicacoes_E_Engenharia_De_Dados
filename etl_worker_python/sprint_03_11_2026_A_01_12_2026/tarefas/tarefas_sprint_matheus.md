# Sprint Moringa ETL — planejamento e tarefas de Matheus

**Período:** 03/11/2026 a 01/12/2026, inclusive. Datas no fuso America/Sao_Paulo.  
**Responsável pela execução:** Matheus, estudante de Análise e Desenvolvimento de Sistemas.  
**Disponibilidade:** até 20 horas por semana, incluindo reuniões, estudo necessário, implementação, testes e documentação.  
**Objetivo:** evoluir o ETL local para uma versão mínima operacional na nuvem, com PostgreSQL do Supabase sem TimescaleDB, Firebase preservado, duas fontes públicas e dados documentados para consumo por IA.

O ano de 2026 é uma premissa baseada na data do pedido. Este documento planeja trabalho futuro: não representa migração, implantação ou testes já executados. O conteúdo do ZIP foi tratado como evidência técnica, não como instruções para executar o projeto. A análise foi estática, sem acessar Firebase ou banco e sem usar as credenciais do anexo.

## Link do drive para acesso ao código: https://drive.google.com/file/d/1YhjvqUoyqBlEqbu5IeLoPLUpDiKYMQLh/view?usp=sharing
## Linke para o repositório com o etl: https://github.com/GpadsTech/etl_worker_python/

## 1. O que existe e o que falta

Base analisada: `etl_worker_python_v2 (1).zip`, diretório interno `etl_worker_pythonn/etl_worker_python`.

| Evidência no código | Situação observada | Consequência para a sprint |
| --- | --- | --- |
| `app/main.py` e `app/worker/etl_worker.py` | Loop contínuo com intervalo configurável. Leitura completa de sensores e boias com `.get()` em cada ciclo | Implementar paginação, avanço incremental e retomada |
| `app/transformer/mapper.py` e `normalize.py` | Mapeamento de campos. Boias têm conversão explícita para números. Sensores não têm a mesma conversão | Unificar validação e tratamento de ausências |
| `app/transformer/timestamp.py` | Datas sem informação de fuso | Confirmar fuso de origem e persistir instantes em UTC |
| `app/repository/*.py` | SQL parametrizado, `ON CONFLICT DO NOTHING`, retorno de inserção e commit por registro | Preservar idempotência e testar gravação em lotes |
| `app/sql/schema.sql` e `init_timescale.sql` | Duas tabelas, índices por equipamento/tempo e chamadas a `create_hypertable`/`CREATE EXTENSION` | Criar migrações portáveis para PostgreSQL puro |
| `app/database/` e `app/repository/` | Duas implementações de repositórios. Worker usa `repository`, bootstrap usa `database` | Escolher uma implementação e corrigir os imports |
| `app/firebase/readers/listener.py` | Apenas esqueleto, sem escuta implementada | Usar coleta periódica incremental nesta sprint |
| `tests/` | Testes de transformação e integração, vários com serviços reais | Separar testes offline dos testes com infraestrutura descartável |
| Arquivos de configuração no pacote | Há `.env`, ambiente virtual e arquivo de credencial Firebase | Não publicar o ZIP como repositório. Revisar segredos, substituir por exemplos e tratar a chave com o mantenedor |

O contexto confirmado é monitoramento ambiental: temperatura, umidade, chuva, vento e pressão nos sensores, além de pH e sinais elétricos nas boias. A localização real dos equipamentos e as unidades de parte dos campos ainda precisam de confirmação. Não transformar tensão de turbidez em NTU, nem tensão UV em índice UV, sem calibração. O código não comprova desempenho em produção, volume real, backups ou operação na nuvem.

## 2. Capacidade e calendário

Premissa de planejamento: 4 horas por dia útil. Não há trabalho previsto nos fins de semana ou em 20/11, feriado nacional. O feriado de 15/11 cai no domingo. Feriados locais, aulas e provas ainda precisam ser informados no início. O plano não presume compensação de feriados.

| Janela | Capacidade | Cerimônias | Trabalho técnico | Reserva | Evidência de conclusão |
| --- | ---: | ---: | ---: | ---: | --- |
| 03–06/11 | 16 h | 1,50 h | 14,50 h | 0 h | Diagnóstico, decisão de hospedagem, SQL portável e contrato dos conectores |
| 09–13/11 | 20 h | 2,50 h | 17,50 h | 0 h | Migração ensaiada, Firebase incremental e medição inicial |
| 16–19/11 | 16 h | 2,25 h | 13,75 h | 0 h | INMET, decisão APAC, qualidade e interface de dados para IA |
| 23–27/11 | 20 h | 2,50 h | 14,50 h | 3 h | Segunda fonte, implantação e testes de recuperação |
| 30/11–01/12 | 8 h | 1,75 h | 3,25 h | 3 h | Correções finais, manual e demonstração |
| **Total** | **80 h** | **10,50 h** | **63,50 h** | **6 h** | **80 h, sem horas extras** |

As horas técnicas incluem pesquisa e aprendizagem. O mentor revisa decisões e desbloqueia problemas, mas sua disponibilidade não faz parte das 80 horas de Matheus. Trabalhar com no máximo uma tarefa principal de implementação por vez e abrir PRs pequenos.

### Cerimônias e marcos

- **Daily:** 15 minutos em cada um dos 20 dias úteis do período, total de 5 h. Matheus informa o que concluiu, o próximo passo e bloqueios. Após 45 minutos sem avanço em um bloqueio, registra evidências e pede apoio ao mentor.
- **Review de código nas segundas:** 09, 16, 23 e 30/11, 45 minutos cada, total de 3 h. Revisar PRs, bugs, consultas e resultados de testes. Correções entram nas tarefas ou na reserva, não em horas extras.
- **Reunião de sprint nas terças:** 03, 10, 17 e 24/11 e 01/12, 30 minutos cada, total de 2,5 h. 03/11: planejamento e acessos. 10/11: checkpoint da migração. 17/11: fontes e qualidade. 24/11: implantação e escopo final. 01/12: demonstração, aceite e retrospectiva breve.
- As cerimônias contam separadamente no orçamento. O horário exato fica a combinar. Nas segundas e terças, reservar o tempo da reunião dentro das 4 horas do dia.
- **09/11:** revisão do SQL e contrato de dados. **16/11:** revisão da migração e desempenho. **23/11:** revisão das fontes e interface de IA. **30/11:** revisão final de operação e bugs.

## 3. Escopo e decisões de arquitetura

### Compromisso da sprint

1. Manter sensores e boias do Firebase funcionando no PostgreSQL puro do Supabase.
2. Fazer carga inicial controlada e ciclos incrementais com retomada, idempotência, validação e rastreabilidade.
3. Integrar INMET por arquivos históricos oficiais e uma segunda fonte pública: APAC se viável, NASA POWER como alternativa planejada.
4. Hospedar o código em repositório privado e executar o ETL em serviço adequado, com Render como primeira opção de avaliação.
5. Entregar uma view SQL versionada e um exemplo Python de leitura com usuário somente leitura para IA. Não inclui treinar um modelo.

### PostgreSQL sem TimescaleDB

- Remover a dependência da extensão e as chamadas de hypertable. Não há continuous aggregates ou compressão implementados nos scripts inspecionados. Não prometer equivalência automática às funções da extensão.
- Começar com tabelas comuns, índices compostos por equipamento e tempo, consultas limitadas por período e inserções em lote. Medir antes de adotar particionamento.
- Se as medições justificarem, propor particionamento mensal nativo por tempo e rotina de criação de partições. Isso fica como melhoria opcional, pois adiciona manutenção. Qualquer chave única de tabela particionada deve incluir a chave de partição.
- Usar `TIMESTAMPTZ` para instantes de observação e ingestão. Confirmar o fuso dos registros legados antes de converter. Um campo já em UTC não pode receber uma segunda conversão.
- Preservar o dado original e a chave da origem. Uma chave de evento deve distinguir fonte, equipamento e registro. A chave atual `(firebase_id, timestamp)` precisa de teste de colisão entre equipamentos e de política para correções.
- Conectar com TLS e privilégios mínimos. Para execução persistente, testar conexão direta quando a rede suportar, ou pooler em modo sessão se necessário por IPv4. Copiar os parâmetros do projeto no Supabase, sem deduzir hostname.

### Hospedagem e estado

- **Código:** repositório privado Git com documentação, dependências reprodutíveis e exemplos sem segredos.
- **Banco e estado durável:** Supabase PostgreSQL, incluindo checkpoints, histórico de execução e rejeições. Arquivos brutos selecionados podem usar armazenamento de objetos com acesso restrito e retenção documentada.
- **Execução:** Render Background Worker se mantiver o loop contínuo. Render Cron Job se criar comando de execução única, com término e código de saída correto. Avaliar um único comando por ciclo que isole falhas por fonte e respeite a cadência de cada conector.
- Cron não oferece disco persistente e tem limite de 12 h por execução. O loop atual não deve ser cadastrado diretamente como cron. Cron tem cobrança mínima informada pelo provedor, portanto a pesquisa deve registrar preços atuais, limites, região e orçamento estimado, sem presumir gratuidade.
- A decisão final de serviço, cadência e custo cabe à liderança na reunião de 10/11. Matheus prepara a recomendação em 06/11. A liderança deve liberar contas, acessos e orçamento para o teste em nuvem.

### Fontes públicas selecionadas

| Fonte | Uso no projeto e acesso verificado | Entrega e limite |
| --- | --- | --- |
| **INMET** | Histórico oficial anual das estações automáticas, em pacotes disponíveis no portal. Temperatura, umidade, pressão, vento e precipitação, conforme o arquivo | Conector de ZIP/CSV oficial com recorte de 1 a 3 estações próximas aos equipamentos e 30 a 90 dias de dados disponíveis. Verificar arquivo e atualização real. Não prometer tempo real |
| **APAC** | Geoportal ArcGIS REST, camada de chuva acumulada, com campos de estação, coordenadas, última leitura e janelas de acumulação. A camada informa JSON/GeoJSON e paginação | Investigar acesso, atualização, identificador, unidades e condições de uso em 2 h. Se viável, integrar um recorte de Pernambuco. Capturas sucessivas formam histórico próprio a partir da coleta, sem inventar passado |
| **NASA POWER** | API oficial diária em JSON/CSV com parâmetros meteorológicos e solares por coordenadas | Alternativa à APAC dentro das mesmas 5 h. Usar 1 a 3 coordenadas e recorte histórico pequeno. Identificar dados em grade/modelados, resolução e referência temporal, sem apresentá-los como sensores locais |
| **ANA HidroWebservice** | Chuva, nível e vazão são pertinentes ao monitoramento hídrico. Serviço atual tem autenticação e fluxo de solicitação de acesso | Backlog. A liderança pode solicitar acesso, se desejar. Não é dependência da entrega e Matheus não deve construir sobre o serviço legado |

Tratei “inmp” como provável referência ao **INMET**, por coerência com meteorologia e APAC. Confirmar no kickoff. Pernambuco é um recorte inicial sugerido pela APAC, não uma localização comprovada do Moringa. Se os equipamentos estiverem fora de PE, priorizar a NASA POWER como segunda fonte na mesma tarefa.

**Regra de decisão em 19/11:** se a APAC não fornecer resposta utilizável, data de observação confiável, identificador estável ou condições de acesso adequadas, documentar a evidência e substituir pela NASA POWER. Entregar INMET + APAC **ou** INMET + NASA POWER. Uma terceira integração não entra no compromisso.

## 4. Tarefas detalhadas

Os prazos abaixo são de conclusão. Todas as estimativas incluem estudo, implementação, testes específicos e atualização da documentação da própria tarefa. Critérios de aceite são evidências a produzir, não resultados já atingidos.

### T01 — Diagnóstico, acessos e preparação do repositório

- **Prazo:** 04/11/2026.
- **Quantidade prevista de horas gastas:** 3,5 h.
- **A quem se refere:** Matheus, com apoio do mentor e do mantenedor das credenciais.
- **Descrição detalhada:** mapear o caminho executado por `main.py`, distinguir repositórios ativos dos legados e registrar os componentes implementados. Confirmar versão de Python, volume aproximado, localização dos equipamentos, fuso original, unidades e disponibilidade de banco de teste. Criar ambiente limpo sem reutilizar o `.venv` do ZIP. Revisar `.gitignore` e produzir `.env.example` sem valores reais. O anexo contém um arquivo de chave Firebase: pedir ao mantenedor que confirme sua validade e revogue/substitua a chave se ativa, antes da implantação, sem copiar o segredo para PRs ou documentação. Definir quais testes são offline e quais dependem de serviços.
- **Dependências:** acessos fornecidos pela liderança, sem bloquear o inventário offline.
- **Aceite:** checklist de diagnóstico, fluxo documentado e configuração de exemplo. Nenhum segredo ou ambiente virtual no conteúdo a publicar. Lista de pendências com responsável.

### T02 — Pesquisa e decisão de hospedagem

- **Prazo:** 06/11/2026. Recomendação para decisão da liderança em 10/11.
- **Quantidade prevista de horas gastas:** 2 h.
- **A quem se refere:** Matheus. Liderança aprova serviço e orçamento.
- **Descrição detalhada:** comparar Render Background Worker e Cron Job conforme loop contínuo ou execução única. Registrar comando de início, cron em UTC se aplicável, região, necessidade de rede IPv4/IPv6, TLS, segredos, logs, reinício, custos atuais e ausência de persistência local no cron. Verificar se os limites de compute e Supabase atendem o recorte inicial. Separar onde fica o código, onde roda o processo e onde ficam dados/estado. Registrar decisão curta em `docs/adr-hospedagem.md`, com alternativa caso Render não seja viável.
- **Dependências:** T01 e acesso à documentação oficial.
- **Aceite:** recomendação com links, data de consulta, custo estimado baseado na cadência proposta e lista de liberações necessárias. Não assumir plano gratuito suficiente.

### T03 — Migrações para PostgreSQL puro e conexão Supabase

- **Prazo:** 06/11/2026.
- **Quantidade prevista de horas gastas:** 6 h.
- **A quem se refere:** Matheus, com revisão SQL do mentor em 09/11.
- **Descrição detalhada:** criar migrações SQL versionadas sem `CREATE EXTENSION timescaledb` e `create_hypertable`. Preservar sensores e boias, ajustar instantes para `TIMESTAMPTZ` após confirmar a origem e revisar as chaves de idempotência por equipamento. Manter índices úteis por equipamento/tempo e evitar índices redundantes sem medição. Adaptar `DatabaseConnection` para configuração por ambiente, TLS, timeout e conexão do Supabase. Definir permissões de escrita do ETL e leitura da IA. Manter tabelas privadas ou políticas RLS adequadas caso expostas pela Data API.
- **Dependências:** T01. Implementação e validação local podem avançar antes da liberação do Supabase.
- **Aceite:** migrações sobem um PostgreSQL vazio sem TimescaleDB. Sensores e boias inserem e consultam dados. Falha de conexão produz erro claro sem revelar senha. SQL e teste de conexão ficam prontos para revisão.

### T04 — Ensaio de migração e reconciliação dos dados

- **Prazo:** 11/11/2026.
- **Quantidade prevista de horas gastas:** 5 h.
- **A quem se refere:** Matheus. Liderança libera o ambiente e define a janela de mudança.
- **Descrição detalhada:** preparar exportação de dados do banco local e carga no destino, sem restaurar estruturas dependentes de TimescaleDB. Ensaiar com recorte pequeno e depois com o volume acordado. Registrar contagens por tabela/equipamento/dia, menor e maior instante, nulos e amostra de valores. Documentar backup antes da mudança, reconciliação do delta, pausa/retomada do coletor e rollback para o ambiente anterior. Evitar dois processos escrevendo o mesmo fluxo durante o corte.
- **Dependências:** T03 revisada e acesso aos dados de teste.
- **Aceite:** relatório de reconciliação sem diferença inexplicada no recorte e procedimento reproduzível de migração e retorno. A migração completa depende de volume e acessos confirmados, e deve ter seu recorte explicitado.

### T05 — Firebase incremental com checkpoint e retomada

- **Prazo:** 12/11/2026.
- **Quantidade prevista de horas gastas:** 6 h.
- **A quem se refere:** Matheus, com apoio do mentor na definição do cursor.
- **Descrição detalhada:** substituir a leitura completa repetida por páginas por equipamento e cursor persistido no PostgreSQL. Escolher chave ordenável ou campo de tempo apenas após verificar a estrutura real e os índices permitidos no Firebase. Implementar bootstrap paginado, avanço incremental e uma janela configurável de reconsulta para dados atrasados. Definir como correções antigas entram por reprocessamento explícito. Confirmar a transação antes de avançar o checkpoint. Rejeições devem ficar registradas de forma durável antes de avançar, para evitar perda silenciosa.
- **Dependências:** T03 e T04 para verificar paridade.
- **Aceite:** testes com páginas, registros fora de ordem, repetição de página e interrupção entre gravação e checkpoint. Reinício não perde registros nem cria duplicatas. Documentar a janela de atraso e a política de correção histórica.

### T06 — Gravação em lotes e comparação de desempenho

- **Prazo:** 13/11/2026.
- **Quantidade prevista de horas gastas:** 3 h.
- **A quem se refere:** Matheus. Mentor avalia os resultados em 16/11.
- **Descrição detalhada:** aplicar inserção em lote na implementação ativa dos repositórios e corrigir imports do bootstrap. Usar transações curtas, tamanho de lote configurável e tratamento de registro inválido sem descartar o restante. Medir cenário de referência com o código atual e código proposto usando o mesmo conjunto e ambiente comparável. Medir o Supabase separadamente para mostrar o efeito da rede. Coletar duração, registros/s, memória aproximada e planos das consultas por equipamento/período.
- **Dependências:** T04 e T05.
- **Aceite:** relatório com lote de referência proposto de 10 mil registros e consulta de 30 dias, ajustado ao volume real em 04/11. Meta proposta: throughput no destino igual ou superior à referência local e p95 de consulta até 1 s, com 20 repetições aquecidas e condições registradas. Se não atingir, documentar gargalo e decidir ajuste com o mentor em 16/11. Medições de rede não são diretamente equivalentes ao teste local. Não declarar produtividade mantida sem evidência.

### T07 — Contrato comum e estrutura dos conectores

- **Prazo:** 06/11/2026.
- **Quantidade prevista de horas gastas:** 3 h.
- **A quem se refere:** Matheus, com revisão do mentor em 09/11.
- **Descrição detalhada:** definir interface simples de conector que retorne páginas ou lotes e metadados da fonte. Manter Firebase compatível e adicionar tabelas de fontes, estações, observações externas, execuções, checkpoints e rejeições, com o mínimo necessário. Separar aquisição do formato bruto, transformação e carga. Para observações externas, registrar fonte, estação ou ponto, variável, unidade, intervalo observado, referência temporal, valor, qualidade, ingestão e versão do conector. Diferenciar observação de estação de dado em grade e preservar o identificador original.
- **Dependências:** T01. Desenvolvimento em paralelo lógico com o desenho de T03, executado sequencialmente por Matheus.
- **Aceite:** contrato e exemplo offline revisáveis, sem obrigar fontes externas a preencher `firebase_id`. Chave única documentada e teste de colisão entre fontes.

### T08 — Conector INMET com recorte histórico

- **Prazo:** 18/11/2026.
- **Quantidade prevista de horas gastas:** 6 h.
- **A quem se refere:** Matheus.
- **Descrição detalhada:** localizar pacote anual oficial, baixar de forma controlada e processar ZIP/CSV de 1 a 3 estações relevantes. Identificar cabeçalho de metadados, codificação, separador, decimais, ausência de dados e referência de hora pelo arquivo real. Mapear variáveis necessárias sem adivinhar unidades. Limitar a carga a 30–90 dias disponíveis. Guardar origem, data do download e hash do arquivo. Evitar baixar e reprocessar o mesmo pacote sem mudança. Atualização histórica ocorre quando a fonte publica um novo arquivo.
- **Dependências:** T03 e T07. Usar recorte geográfico confirmado em T01.
- **Aceite:** execução repetível com pelo menos uma estação e uma variável real, data e unidade comprovadas. Mesma carga duas vezes não duplica dados. Fixture reduzida testa um registro válido, ausente e malformado. Manual explica a defasagem e como atualizar o arquivo.

### T09 — Investigação limitada da APAC e decisão de alternativa

- **Prazo:** 19/11/2026.
- **Quantidade prevista de horas gastas:** 2 h.
- **A quem se refere:** Matheus. Mentor valida a escolha.
- **Descrição detalhada:** consultar o Geoportal APAC e a operação `query` da camada de chuva acumulada. Verificar acesso efetivo, paginação, identificador estável, unidades, horário da última leitura, cobertura de PE e termos de uso. O catálogo mostra `horas_24`, mas isso não comprova que a resposta atual esteja completa ou atualizada. Capturar uma amostra sanitizada e documentar as janelas acumuladas. Não somar janelas sobrepostas nem tratar cada captura como uma nova observação de chuva.
- **Dependências:** T07 e recorte territorial de T01.
- **Aceite:** decisão registrada até 19/11: APAC aprovada ou NASA POWER como substituta. Se houver bloqueio, não ultrapassar as 2 h de investigação. As 5 h de T10 seguem para a alternativa.

### T10 — Segunda fonte pública: APAC ou NASA POWER

- **Prazo:** 25/11/2026.
- **Quantidade prevista de horas gastas:** 5 h.
- **A quem se refere:** Matheus.
- **Descrição detalhada:** implementar apenas a fonte escolhida em T09. Para APAC, consultar recorte de estações, paginar quando necessário, preservar a data da observação e a janela de acumulação, registrar revisões e manter histórico a partir da primeira coleta. Para NASA POWER, consultar a API diária por 1–3 coordenadas e 30–90 dias, selecionar poucos parâmetros, pedir referência UTC quando aplicável e preservar metadados, resolução e valores de ausência. Usar timeout, tentativas limitadas com espera progressiva e cache. Respeitar limites e condições da fonte.
- **Dependências:** T07, T09 e as regras de qualidade de T11.
- **Aceite:** carga real no Supabase, fixture offline e reexecução idempotente. Uma fonte falhando não impede a outra nem o Firebase. Consulta mostra proveniência e cobertura. A alternativa mantém o mesmo prazo e orçamento.

### T11 — Qualidade, unidades e rastreabilidade

- **Prazo:** 19/11/2026.
- **Quantidade prevista de horas gastas:** 4 h, sendo 3,5 h em 09–13/11 e 0,5 h em 16–19/11.
- **A quem se refere:** Matheus. Mentor confirma regras de domínio.
- **Descrição detalhada:** aplicar conversão numérica consistente para sensores e boias, rejeitar ou sinalizar datas e números inválidos e preservar ausências como nulos. Criar regras documentadas para umidade, pH e chuva apenas quando unidade e semântica forem confirmadas. Manter valores suspeitos rastreáveis e separar erro de parsing de alerta físico. Registrar fonte, equipamento, observação, ingestão, execução e motivo de rejeição. Incluir metadados de localização e calibração conhecida, ou marcar desconhecido.
- **Dependências:** T03 e T07. As integrações posteriores reutilizam essas regras.
- **Aceite:** casos de nulo, string numérica, número inválido, data sem fuso e observação suspeita têm comportamento explícito. Logs e tabela de rejeições permitem investigar sem expor credenciais. Tensão elétrica permanece em volts enquanto não houver calibração.

### T12 — Interface de leitura para IA e dicionário de dados

- **Prazo:** 19/11/2026. Validação com a segunda fonte até 27/11.
- **Quantidade prevista de horas gastas:** 4 h.
- **A quem se refere:** Matheus. Liderança ou responsável pela IA valida o contrato.
- **Descrição detalhada:** criar view SQL versionada para observações ambientais com fonte, localização, equipamento/ponto, variável, unidade, início e fim do período, valor, tipo de dado, qualidade e instante de ingestão. Unificar a leitura sem misturar frequência horária, diária e chuva acumulada como se fossem equivalentes. Criar usuário somente leitura e exemplo Python parametrizado por período e estação, com ordenação estável e limite de linhas. Documentar junções por proximidade e tempo, cobertura, nulos e defasagem, sem construir uma junção automática sem critério de domínio.
- **Dependências:** T07, T08 e T11. T10 preenche o mesmo contrato posteriormente.
- **Aceite:** consulta de exemplo funciona com Firebase e INMET e permite adicionar T10 sem mudar a interface. A conta de leitura não consegue gravar. Instruções distinguem momento observado de momento em que o dado ficou disponível, para reduzir vazamento temporal em treinamento. Não inclui API pública ou treinamento de modelo.

### T13 — Implantação do ETL e operação na nuvem

- **Prazo:** 26/11/2026.
- **Quantidade prevista de horas gastas:** 5 h.
- **A quem se refere:** Matheus. Liderança fornece contas, orçamento e permissões.
- **Descrição detalhada:** aplicar a decisão de hospedagem aprovada em T02. Criar Dockerfile ou configuração equivalente, comando de início e encerramento correto. Se cron, expor execução única e códigos de saída. Se worker, tratar parada e reinício. Configurar segredos no provedor, conexão TLS e estado durável no Supabase. Evitar sobreposição entre bootstrap e ciclos regulares. Registrar por fonte início/fim, quantidade recebida, inserida, ignorada e rejeitada, duração e última execução bem-sucedida. Definir como detectar falha e ausência inesperada de atualizações conforme a cadência de cada origem.
- **Dependências:** T02, T04, T05 e T08. Adicionar T10 ao mesmo mecanismo de execução.
- **Aceite:** implantação reproduzível e uma execução real bem-sucedida por fonte no destino. Reiniciar o serviço preserva checkpoints. Nenhuma dependência de arquivo local durável. Iniciar observação operacional em 26/11 para conferir os ciclos até 30/11.

### T14 — Testes de regressão, falha e recuperação

- **Prazo:** 27/11/2026. Conferência da operação em 30/11 na revisão.
- **Quantidade prevista de horas gastas:** 5 h, sendo 1,25 h em 16–19/11 e 3,75 h em 23–27/11.
- **A quem se refere:** Matheus, com revisão do mentor.
- **Descrição detalhada:** separar testes offline dos que usam banco isolado. Cobrir duplicação, correção de observação, colisão entre equipamentos, lotes inválidos, datas, paginação e falha de rede. Simular queda após commit e antes do checkpoint, timeout de fonte e indisponibilidade do banco. Verificar que retries têm limite, que erros não geram loops silenciosos e que uma fonte não bloqueia as demais. Reexecutar a medição de T06 após as integrações e registrar eventuais desvios. Conferir logs dos ciclos esperados da implantação, respeitando a periodicidade real de cada fonte.
- **Dependências:** T05, T06, T08, T10, T11, T12 e T13 para o aceite completo. Preparar fixtures e cenários antes de T10/T13.
- **Aceite:** testes críticos passam sem acesso involuntário ao ambiente real. Relatório registra reconciliação, desempenho e recuperação. Evidência de ao menos dois ciclos agendados bem-sucedidos onde a cadência permitir. Falhas conhecidas têm impacto e ação registrados.

### T15 — Manual de operação e demonstração de entrega

- **Prazo:** 01/12/2026.
- **Quantidade prevista de horas gastas:** 4 h, sendo 0,75 h em 23–27/11 e 3,25 h em 30/11–01/12.
- **A quem se refere:** Matheus. Liderança e responsável pela IA participam do aceite.
- **Descrição detalhada:** consolidar README, variáveis de exemplo, desenho do fluxo, decisão de hospedagem, migrações, dicionário, fontes e limitações. Escrever passo a passo para carga inicial, retomada, reprocessamento por período, consulta da IA, verificação de saúde e rollback. Preparar demonstração curta: consultar dados de cada fonte, repetir carga sem duplicar, mostrar retomada e executar a leitura com conta restrita. Registrar pendências e sugestões para a próxima sprint. A apresentação de planejamento entregue junto deste arquivo serve como abertura da sprint, e o roteiro de demo serve ao encerramento.
- **Dependências:** T13 e T14, além da revisão de 30/11.
- **Aceite:** outra pessoa consegue seguir o manual. Demonstração e checklist de aceite ficam registrados. A reunião de 01/12 usa os 30 minutos já contabilizados em cerimônias, sem duplicar a estimativa.

### T16 — Dailies, reviews e reuniões de sprint

- **Prazo:** recorrente entre 03/11 e 01/12/2026.
- **Quantidade prevista de horas gastas:** 10,5 h.
- **A quem se refere:** Matheus. Mentor participa das reviews e liderança das reuniões de sprint.
- **Descrição detalhada:** participar das cerimônias nas datas e durações da seção 2, preparar rapidamente o status a partir do quadro de tarefas e registrar decisões de escopo e bloqueios. Não ampliar o escopo sem ajustar a capacidade.
- **Aceite:** 20 dailies, quatro reviews de segunda e cinco reuniões de terça previstas, com registros objetivos das decisões. Ausências devem reduzir a capacidade disponível.

### T17 — Reserva para bugs e estabilização

- **Prazo:** 3 h reservadas para 23–27/11 e 3 h para 30/11–01/12.
- **Quantidade prevista de horas gastas:** 6 h reservadas, usadas apenas se necessário.
- **A quem se refere:** Matheus, com priorização do mentor.
- **Descrição detalhada:** corrigir falhas encontradas nas reviews, na migração e na operação. Priorizar perda de dados, duplicação, segurança de acesso, retomada e consulta da IA. Abrir subtarefas com evidência, causa e verificação da correção. A reserva não é uma promessa de funcionalidade adicional.
- **Aceite:** bugs críticos resolvidos e verificados. Reserva não utilizada permanece livre. Qualquer demanda acima de 80 h exige troca explícita de escopo.

## 5. Resumo para o quadro de tarefas

| ID | Título | Prazo | Horas | Responsável |
| --- | --- | --- | ---: | --- |
| T01 | Diagnóstico e repositório | 04/11 | 3,5 | Matheus |
| T02 | Pesquisa de hospedagem | 06/11 | 2 | Matheus |
| T03 | PostgreSQL puro e conexão | 06/11 | 6 | Matheus |
| T04 | Ensaio de migração | 11/11 | 5 | Matheus |
| T05 | Firebase incremental | 12/11 | 6 | Matheus |
| T06 | Lotes e desempenho | 13/11 | 3 | Matheus |
| T07 | Contrato dos conectores | 06/11 | 3 | Matheus |
| T08 | INMET | 18/11 | 6 | Matheus |
| T09 | Investigação APAC | 19/11 | 2 | Matheus |
| T10 | APAC ou NASA POWER | 25/11 | 5 | Matheus |
| T11 | Qualidade e rastreabilidade | 19/11 | 4 | Matheus |
| T12 | Interface para IA | 19/11 | 4 | Matheus |
| T13 | Implantação | 26/11 | 5 | Matheus |
| T14 | Testes e recuperação | 27/11 | 5 | Matheus |
| T15 | Manual e demonstração | 01/12 | 4 | Matheus |
| **Subtotal técnico** | | | **63,5** | |
| T16 | Cerimônias | Recorrente | 10,5 | Matheus |
| T17 | Reserva | Até 01/12 | 6 | Matheus |
| **Total** | | | **80** | |

### Distribuição técnica para não exceder a capacidade semanal

| Janela | Alocação técnica |
| --- | --- |
| 03–06/11 | T01 3,5 h + T02 2 h + T03 6 h + T07 3 h = 14,5 h |
| 09–13/11 | T04 5 h + T05 6 h + T06 3 h + T11 3,5 h = 17,5 h |
| 16–19/11 | T08 6 h + T09 2 h + T11 0,5 h + T12 4 h + T14 1,25 h = 13,75 h |
| 23–27/11 | T10 5 h + T13 5 h + T14 3,75 h + T15 0,75 h = 14,5 h |
| 30/11–01/12 | T15 3,25 h |

A ordem dos IDs agrupa assuntos. A execução segue dependências e a distribuição acima. Dentro da primeira janela, T03 e T07 devem alinhar o desenho antes da implementação final.

## 6. Critérios de pronto para 01/12

- [ ] PostgreSQL do Supabase funciona sem TimescaleDB e migrações são reproduzíveis.
- [ ] Sensores e boias preservam os dados do recorte migrado, com reconciliação documentada.
- [ ] Coleta incremental, reprocessamento e reinício não causam perda nem duplicação nos cenários testados.
- [ ] INMET e APAC, ou INMET e NASA POWER, carregam dados reais com fonte, período, unidade e qualidade.
- [ ] Matheus entrega medições comparáveis e explica qualquer diferença em relação à operação local.
- [ ] Serviço em nuvem executa os ciclos previstos e preserva estado após reinício.
- [ ] Conta da IA lê a view documentada e não consegue escrever.
- [ ] Código, manual, configuração de exemplo e testes críticos estão revisados.
- [ ] Não há bug crítico aberto de perda de dados, duplicação ou exposição de credenciais.
- [ ] A liderança registra aceite, ressalvas e backlog na reunião de 01/12.

**Roteiro de aceite, 30 minutos:** 5 min para objetivo e arquitetura, 10 min para dados de cada fonte e consulta da IA, 5 min para duplicação e retomada, 5 min para desempenho e operação, 5 min para limitações e aceite.

## 7. Dependências, riscos e ajuste de escopo

| Situação | Ação e responsável | Momento de decisão |
| --- | --- | --- |
| Falta de Supabase, Render, banco local ou permissão Firebase | Liderança libera acesso. Matheus usa fixtures e banco local isolado enquanto isso | Kickoff 03/11. Se persistir em 10/11, replanejar o compromisso em nuvem |
| Volume maior que o estimado | Matheus mede e propõe carga em janelas. Mentor define recorte e prazo de migração completa | Até 11/11 |
| Desempenho inferior à referência | Analisar rede, lote, consulta e plano antes de particionar ou contratar capacidade | Review 16/11 |
| APAC indisponível, sem horário confiável ou fora do recorte | Substituir por NASA POWER, sem aumentar T10 | Até 19/11 |
| Regras de unidade, localização ou calibração desconhecidas | Preservar bruto, sinalizar desconhecido e limitar consumo da variável | T01 e review 23/11 |
| Atrasos, prova ou feriado local | Reduzir escopo com a liderança e manter o teto de 20 h/semana | Daily e terça seguinte |

**Ordem de proteção do escopo:** integridade do Firebase e migração, retomada e operação, fontes públicas mínimas, interface documentada para IA. Primeiro retirar melhorias opcionais. Se ainda faltar capacidade, a liderança deve reduzir explicitamente o compromisso, sem registrar entrega parcial como concluída.

**Backlog opcional, sem horas nesta sprint:** integração ANA após acesso, particionamento nativo se comprovadamente necessário, retenção automatizada, agregados materializados, painel de operação, CI ampliada, API de consulta e novos conectores. Não inclui modelo de IA, previsões, dashboard de produto, listener Firebase em tempo real ou carga nacional completa.

## 8. Referências consultadas

Consulta em 03/11/2026. Links oficiais orientam o planejamento. A existência da documentação não substitui testar acesso e qualidade das respostas durante a sprint.

1. [Supabase — conexão ao PostgreSQL](https://supabase.com/docs/guides/database/connecting-to-postgres): conexão direta, IPv4/IPv6, pooler e modos de uso.
2. [Render — Cron Jobs](https://render.com/docs/cronjobs): execução periódica, estado local, limite de duração e cobrança.
3. [Render — tipos de serviço](https://render.com/docs/service-types): worker contínuo e cron periódico.
4. [PostgreSQL — particionamento](https://www.postgresql.org/docs/current/ddl-partitioning.html): particionamento nativo e restrições de chaves únicas.
5. [INMET — dados históricos anuais](https://portal.inmet.gov.br/dadoshistoricos/): arquivos oficiais. O catálogo consultado indicava 2026 até 30/09, reforçando que não se deve prometer atualização em tempo real.
6. [APAC — serviço de chuva acumulada](https://geoportal.apac.pe.gov.br/server/rest/services/met_volume_acumulado_chuva_24hs/MapServer) e [camada 0](https://geoportal.apac.pe.gov.br/server/rest/services/met_volume_acumulado_chuva_24hs/MapServer/0): campos, janelas, JSON/GeoJSON e paginação. A validade da unidade e da resposta precisa de confirmação na investigação.
7. [NASA POWER — API diária](https://power.larc.nasa.gov/docs/services/api/temporal/daily/): séries por coordenada, formatos, resolução e referência UTC/LST.
8. [ANA — documentação da API](https://www.ana.gov.br/hidrowebservice/swagger-ui/index.html) e [aviso sobre migração e solicitação de acesso](https://www.ana.gov.br/telemetria1ws/Mapa.aspx?tp=EstacoesHidrometeorologicas): API autenticada e transição do serviço legado.
9. [Calendário oficial de feriados do Sirc](https://www.sirc.gov.br/feriados-e-indisponibilidades-sirc/): feriados nacionais de novembro de 2026.

## 9. Uso da apresentação no Canva

Importar o arquivo `.pptx` entregue junto deste documento no Canva. Textos e elementos de conteúdo foram mantidos como objetos editáveis, em formato 16:9 e fonte Arial. A importação pode alterar quebras de linha e espaçamento, por isso revisar os slides antes de apresentar. As notas do apresentador contêm referências e detalhes de condução que também estão documentados aqui.
