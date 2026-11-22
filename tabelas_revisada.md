# Sistema de Registro Hospitalar

**Equipe:** Enzo e Arthur Sampaio  
**Modalidade:** Modelo 1  
**Referência de tipos:** MySQL  
**Quantidade:** 50 tabelas

> Projeto acadêmico de modelagem. Utilize apenas dados fictícios. Os campos e relacionamentos abaixo representam uma proposta lógica para fins didáticos.

## Convenções
**PK:** chave primária, identifica cada registro.
**FK:** chave estrangeira, referencia a chave primária de outra tabela.
**UNIQUE:** impede valores repetidos no campo (ou conjunto de campos).
Tipos como `INT`, `VARCHAR`, `DATE`, `DATETIME`, `DECIMAL` e `BOOLEAN` seguem a convenção do MySQL.
Nas tabelas associativas, duas ou mais colunas podem formar uma chave primária composta.

## Visão geral dos módulos

**Pacientes:** paciente, endereco, contato_paciente, responsavel_paciente, convenio
**Funcionários:** funcionario, medico, enfermeiro, cargo, especialidade
**Consultas:** agendamento, consulta, sala_consulta, consulta_sala, retorno_consulta
**Prontuários:** prontuario, diagnostico, prontuario_diagnostico, alergia_paciente, historico_clinico
**Internações:** internacao, leito, quarto, alta_hospitalar, evolucao_enfermagem
**Exames:** tipo_exame, solicitacao_exame, resultado_exame, laboratorio, exame_laboratorio
**Medicamentos:** medicamento, prescricao, item_prescricao, estoque_medicamento, movimentacao_estoque
**Financeiro:** fatura, pagamento, item_fatura, tabela_preco, autorizacao_convenio
**Estrutura:** hospital, unidade, setor, equipamento, manutencao_equipamento
**Administração:** usuario, perfil_acesso, permissao, usuario_perfil, perfil_permissao

## Dicionário de dados e explicação das tabelas

### 1. `paciente` — Pacientes


Guarda os dados principais de cada paciente, como nome, CPF e data de nascimento.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_paciente` | `INT` | PK |
| `nome` | `VARCHAR(120)` | — |
| `cpf` | `CHAR(11)` | UNIQUE |
| `data_nascimento` | `DATE` | — |
| `sexo` | `VARCHAR(20)` | — |
| `id_endereco` | `INT` | FK → endereco.id_endereco |

### 2. `endereco` — Pacientes


Guarda o endereço do paciente, incluindo rua, número, bairro e cidade.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_endereco` | `INT` | PK |
| `logradouro` | `VARCHAR(120)` | — |
| `numero` | `VARCHAR(10)` | — |
| `bairro` | `VARCHAR(60)` | — |
| `cidade` | `VARCHAR(60)` | — |
| `uf` | `CHAR(2)` | — |
| `cep` | `CHAR(8)` | — |

### 3. `contato_paciente` — Pacientes


Registra os telefones, e-mails e outros meios de contato do paciente.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_contato` | `INT` | PK |
| `id_paciente` | `INT` | FK → paciente.id_paciente |
| `telefone` | `VARCHAR(20)` | — |
| `email` | `VARCHAR(120)` | — |
| `tipo_contato` | `VARCHAR(30)` | — |

### 4. `responsavel_paciente` — Pacientes


Guarda os dados de uma pessoa responsável ou de contato do paciente.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_responsavel` | `INT` | PK |
| `id_paciente` | `INT` | FK → paciente.id_paciente |
| `nome` | `VARCHAR(120)` | — |
| `parentesco` | `VARCHAR(40)` | — |
| `telefone` | `VARCHAR(20)` | — |

### 5. `convenio` — Pacientes


Registra os convênios aceitos pelo hospital e seus dados de contato.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_convenio` | `INT` | PK |
| `nome` | `VARCHAR(100)` | — |
| `cnpj` | `CHAR(14)` | UNIQUE |
| `telefone_autorizacao` | `VARCHAR(20)` | — |
| `ativo` | `BOOLEAN` | — |

### 6. `funcionario` — Funcionários


Guarda os dados básicos dos funcionários que trabalham no hospital.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_funcionario` | `INT` | PK |
| `nome` | `VARCHAR(120)` | — |
| `cpf` | `CHAR(11)` | UNIQUE |
| `data_admissao` | `DATE` | — |
| `id_setor` | `INT` | FK → setor.id_setor |
| `id_cargo` | `INT` | FK → cargo.id_cargo |

### 7. `medico` — Funcionários


Armazena as informações profissionais dos funcionários que são médicos.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_medico` | `INT` | PK |
| `id_funcionario` | `INT` | FK → funcionario.id_funcionario |
| `crm` | `VARCHAR(20)` | UNIQUE |
| `id_especialidade` | `INT` | FK → especialidade.id_especialidade |

### 8. `enfermeiro` — Funcionários


Armazena as informações profissionais dos funcionários que são enfermeiros.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_enfermeiro` | `INT` | PK |
| `id_funcionario` | `INT` | FK → funcionario.id_funcionario |
| `coren` | `VARCHAR(20)` | UNIQUE |
| `nivel_formacao` | `VARCHAR(50)` | — |

### 9. `cargo` — Funcionários


Lista os cargos e as funções existentes no hospital.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_cargo` | `INT` | PK |
| `nome_cargo` | `VARCHAR(60)` | — |
| `descricao` | `VARCHAR(180)` | — |

### 10. `especialidade` — Funcionários


Lista as especialidades médicas oferecidas pelo hospital.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_especialidade` | `INT` | PK |
| `nome` | `VARCHAR(80)` | — |
| `descricao` | `VARCHAR(180)` | — |

### 11. `agendamento` — Consultas


Registra as consultas marcadas pelos pacientes e os dados do agendamento.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_agendamento` | `INT` | PK |
| `id_paciente` | `INT` | FK → paciente.id_paciente |
| `id_medico` | `INT` | FK → medico.id_medico |
| `data_hora` | `DATETIME` | — |
| `status` | `VARCHAR(20)` | — |

### 12. `consulta` — Consultas


Guarda as informações do atendimento que foi realizado.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_consulta` | `INT` | PK |
| `id_agendamento` | `INT` | FK → agendamento.id_agendamento |
| `data_inicio` | `DATETIME` | — |
| `data_fim` | `DATETIME` | — |
| `observacao` | `TEXT` | — |

### 13. `sala_consulta` — Consultas


Registra as salas usadas para realizar as consultas.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_sala` | `INT` | PK |
| `codigo` | `VARCHAR(20)` | UNIQUE |
| `andar` | `SMALLINT` | — |
| `id_setor` | `INT` | FK → setor.id_setor |

### 14. `consulta_sala` — Consultas


Faz a ligação entre as consultas e as salas onde elas acontecem.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_consulta` | `INT` | PK, FK → consulta.id_consulta |
| `id_sala` | `INT` | PK, FK → sala_consulta.id_sala |

### 15. `retorno_consulta` — Consultas


Registra quando o paciente precisa voltar para uma nova consulta.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_retorno` | `INT` | PK |
| `id_consulta_origem` | `INT` | FK → consulta.id_consulta |
| `data_prevista` | `DATE` | — |
| `motivo` | `VARCHAR(180)` | — |

### 16. `prontuario` — Prontuários


Guarda o prontuário do paciente, reunindo suas informações clínicas.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_prontuario` | `INT` | PK |
| `id_paciente` | `INT` | FK → paciente.id_paciente, UNIQUE |
| `data_abertura` | `DATE` | — |
| `observacoes_gerais` | `TEXT` | — |

### 17. `diagnostico` — Prontuários


Lista os diagnósticos que podem ser associados aos prontuários.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_diagnostico` | `INT` | PK |
| `codigo_cid` | `VARCHAR(10)` | — |
| `descricao` | `VARCHAR(180)` | — |
| `categoria` | `VARCHAR(60)` | — |

### 18. `prontuario_diagnostico` — Prontuários


Liga os diagnósticos ao prontuário e registra o médico responsável.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_prontuario` | `INT` | PK, FK → prontuario.id_prontuario |
| `id_diagnostico` | `INT` | PK, FK → diagnostico.id_diagnostico |
| `data_registro` | `DATE` | — |
| `id_medico` | `INT` | FK → medico.id_medico |

### 19. `alergia_paciente` — Prontuários


Registra as alergias conhecidas do paciente e as reações que elas causam.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_alergia` | `INT` | PK |
| `id_prontuario` | `INT` | FK → prontuario.id_prontuario |
| `substancia` | `VARCHAR(100)` | — |
| `reacao` | `VARCHAR(180)` | — |
| `gravidade` | `VARCHAR(20)` | — |

### 20. `historico_clinico` — Prontuários


Guarda as anotações feitas ao longo do acompanhamento clínico.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_historico` | `INT` | PK |
| `id_prontuario` | `INT` | FK → prontuario.id_prontuario |
| `id_consulta` | `INT` | FK → consulta.id_consulta |
| `data_registro` | `DATETIME` | — |
| `descricao` | `TEXT` | — |

### 21. `internacao` — Internações


Registra a entrada, a saída e o motivo da internação do paciente.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_internacao` | `INT` | PK |
| `id_paciente` | `INT` | FK → paciente.id_paciente |
| `data_entrada` | `DATETIME` | — |
| `data_saida` | `DATETIME` | — |
| `motivo` | `VARCHAR(180)` | — |
| `id_leito` | `INT` | FK → leito.id_leito |

### 22. `leito` — Internações


Guarda os dados dos leitos e informa a situação de cada um.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_leito` | `INT` | PK |
| `codigo` | `VARCHAR(20)` | UNIQUE |
| `id_quarto` | `INT` | FK → quarto.id_quarto |
| `status` | `VARCHAR(20)` | — |

### 23. `quarto` — Internações


Registra os quartos do hospital e o setor ao qual pertencem.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_quarto` | `INT` | PK |
| `numero` | `VARCHAR(10)` | — |
| `id_setor` | `INT` | FK → setor.id_setor |
| `tipo` | `VARCHAR(40)` | — |

### 24. `alta_hospitalar` — Internações


Guarda as informações da alta do paciente e as orientações recebidas.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_alta` | `INT` | PK |
| `id_internacao` | `INT` | FK → internacao.id_internacao, UNIQUE |
| `data_hora` | `DATETIME` | — |
| `orientacoes` | `TEXT` | — |
| `id_medico` | `INT` | FK → medico.id_medico |

### 25. `evolucao_enfermagem` — Internações


Registra as anotações feitas pela equipe de enfermagem durante a internação.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_evolucao` | `INT` | PK |
| `id_internacao` | `INT` | FK → internacao.id_internacao |
| `id_enfermeiro` | `INT` | FK → enfermeiro.id_enfermeiro |
| `data_hora` | `DATETIME` | — |
| `anotacao` | `TEXT` | — |

### 26. `tipo_exame` — Exames


Lista os tipos de exames disponíveis e as informações sobre cada um.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_tipo_exame` | `INT` | PK |
| `nome` | `VARCHAR(100)` | — |
| `descricao` | `VARCHAR(180)` | — |
| `preparo` | `TEXT` | — |

### 27. `solicitacao_exame` — Exames


Registra os exames pedidos pelos médicos para os pacientes.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_solicitacao` | `INT` | PK |
| `id_paciente` | `INT` | FK → paciente.id_paciente |
| `id_medico` | `INT` | FK → medico.id_medico |
| `id_tipo_exame` | `INT` | FK → tipo_exame.id_tipo_exame |
| `data_solicitacao` | `DATETIME` | — |
| `status` | `VARCHAR(20)` | — |

### 28. `resultado_exame` — Exames


Guarda o resultado e o laudo de um exame solicitado.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_resultado` | `INT` | PK |
| `id_solicitacao` | `INT` | FK → solicitacao_exame.id_solicitacao, UNIQUE |
| `data_resultado` | `DATETIME` | — |
| `laudo` | `TEXT` | — |
| `arquivo_url` | `VARCHAR(255)` | — |

### 29. `laboratorio` — Exames


Registra os laboratórios que realizam os exames.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_laboratorio` | `INT` | PK |
| `nome` | `VARCHAR(100)` | — |
| `localizacao` | `VARCHAR(120)` | — |
| `telefone` | `VARCHAR(20)` | — |

### 30. `exame_laboratorio` — Exames


Indica em qual laboratório e em que data o exame será realizado.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_solicitacao` | `INT` | PK, FK → solicitacao_exame.id_solicitacao |
| `id_laboratorio` | `INT` | FK → laboratorio.id_laboratorio |
| `data_agendada` | `DATETIME` | — |

### 31. `medicamento` — Medicamentos


Lista os medicamentos utilizados pelo hospital e suas informações.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_medicamento` | `INT` | PK |
| `nome` | `VARCHAR(120)` | — |
| `principio_ativo` | `VARCHAR(120)` | — |
| `concentracao` | `VARCHAR(50)` | — |
| `unidade_medida` | `VARCHAR(30)` | — |

### 32. `prescricao` — Medicamentos


Registra os medicamentos prescritos pelo médico para o paciente.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_prescricao` | `INT` | PK |
| `id_paciente` | `INT` | FK → paciente.id_paciente |
| `id_medico` | `INT` | FK → medico.id_medico |
| `data_prescricao` | `DATETIME` | — |
| `observacao` | `TEXT` | — |

### 33. `item_prescricao` — Medicamentos


Mostra quais medicamentos fazem parte de uma prescrição e como devem ser usados.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_item_prescricao` | `INT` | PK |
| `id_prescricao` | `INT` | FK → prescricao.id_prescricao |
| `id_medicamento` | `INT` | FK → medicamento.id_medicamento |
| `dose` | `VARCHAR(50)` | — |
| `frequencia` | `VARCHAR(60)` | — |
| `duracao_dias` | `SMALLINT` | — |

### 34. `estoque_medicamento` — Medicamentos


Controla a quantidade de cada medicamento, seu lote e sua validade.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_estoque` | `INT` | PK |
| `id_medicamento` | `INT` | FK → medicamento.id_medicamento |
| `quantidade` | `INT` | — |
| `lote` | `VARCHAR(40)` | — |
| `validade` | `DATE` | — |

### 35. `movimentacao_estoque` — Medicamentos


Registra as entradas, saídas e alterações na quantidade dos medicamentos.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_movimentacao` | `INT` | PK |
| `id_estoque` | `INT` | FK → estoque_medicamento.id_estoque |
| `tipo_movimento` | `VARCHAR(20)` | — |
| `quantidade` | `INT` | — |
| `data_hora` | `DATETIME` | — |
| `motivo` | `VARCHAR(120)` | — |

### 36. `fatura` — Financeiro


Registra as cobranças dos serviços prestados ao paciente.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_fatura` | `INT` | PK |
| `id_paciente` | `INT` | FK → paciente.id_paciente |
| `id_convenio` | `INT` | FK → convenio.id_convenio |
| `data_emissao` | `DATE` | — |
| `valor_total` | `DECIMAL(10,2)` | — |
| `status` | `VARCHAR(20)` | — |

### 37. `pagamento` — Financeiro


Guarda os pagamentos feitos para quitar uma fatura.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_pagamento` | `INT` | PK |
| `id_fatura` | `INT` | FK → fatura.id_fatura |
| `data_pagamento` | `DATETIME` | — |
| `valor_pago` | `DECIMAL(10,2)` | — |
| `forma_pagamento` | `VARCHAR(30)` | — |

### 38. `item_fatura` — Financeiro


Lista os serviços e valores que fazem parte de uma fatura.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_item_fatura` | `INT` | PK |
| `id_fatura` | `INT` | FK → fatura.id_fatura |
| `descricao` | `VARCHAR(160)` | — |
| `quantidade` | `INT` | — |
| `valor_unitario` | `DECIMAL(10,2)` | — |

### 39. `tabela_preco` — Financeiro


Guarda os preços de referência dos serviços oferecidos pelo hospital.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_preco` | `INT` | PK |
| `codigo_servico` | `VARCHAR(30)` | — |
| `descricao` | `VARCHAR(120)` | — |
| `valor` | `DECIMAL(10,2)` | — |
| `vigencia_inicio` | `DATE` | — |

### 40. `autorizacao_convenio` — Financeiro


Registra os pedidos de autorização enviados aos convênios.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_autorizacao` | `INT` | PK |
| `id_convenio` | `INT` | FK → convenio.id_convenio |
| `id_paciente` | `INT` | FK → paciente.id_paciente |
| `codigo_autorizacao` | `VARCHAR(50)` | — |
| `status` | `VARCHAR(20)` | — |
| `data_solicitacao` | `DATETIME` | — |

### 41. `hospital` — Estrutura


Guarda os dados principais da instituição hospitalar.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_hospital` | `INT` | PK |
| `nome` | `VARCHAR(120)` | — |
| `cnpj` | `CHAR(14)` | UNIQUE |
| `telefone` | `VARCHAR(20)` | — |
| `endereco_texto` | `VARCHAR(180)` | — |

### 42. `unidade` — Estrutura


Registra as unidades que fazem parte do hospital.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_unidade` | `INT` | PK |
| `id_hospital` | `INT` | FK → hospital.id_hospital |
| `nome` | `VARCHAR(100)` | — |
| `endereco_texto` | `VARCHAR(180)` | — |

### 43. `setor` — Estrutura


Lista os setores de cada unidade, como emergência, UTI e pediatria.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_setor` | `INT` | PK |
| `id_unidade` | `INT` | FK → unidade.id_unidade |
| `nome` | `VARCHAR(80)` | — |
| `ramal` | `VARCHAR(10)` | — |

### 44. `equipamento` — Estrutura


Registra os equipamentos utilizados em cada setor do hospital.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_equipamento` | `INT` | PK |
| `id_setor` | `INT` | FK → setor.id_setor |
| `nome` | `VARCHAR(100)` | — |
| `numero_patrimonio` | `VARCHAR(40)` | UNIQUE |
| `status` | `VARCHAR(30)` | — |

### 45. `manutencao_equipamento` — Estrutura


Guarda o histórico das manutenções feitas nos equipamentos.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_manutencao` | `INT` | PK |
| `id_equipamento` | `INT` | FK → equipamento.id_equipamento |
| `data_manutencao` | `DATE` | — |
| `tipo` | `VARCHAR(40)` | — |
| `descricao` | `TEXT` | — |

### 46. `usuario` — Administração


Registra as contas usadas pelos funcionários para entrar no sistema.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_usuario` | `INT` | PK |
| `id_funcionario` | `INT` | FK → funcionario.id_funcionario, UNIQUE |
| `login` | `VARCHAR(60)` | UNIQUE |
| `senha_hash` | `VARCHAR(255)` | — |
| `ativo` | `BOOLEAN` | — |

### 47. `perfil_acesso` — Administração


Lista os perfis que definem o que cada usuário pode acessar.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_perfil` | `INT` | PK |
| `nome` | `VARCHAR(60)` | UNIQUE |
| `descricao` | `VARCHAR(180)` | — |

### 48. `permissao` — Administração


Lista as ações que podem ser permitidas dentro do sistema.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_permissao` | `INT` | PK |
| `codigo` | `VARCHAR(80)` | UNIQUE |
| `descricao` | `VARCHAR(180)` | — |

### 49. `usuario_perfil` — Administração


Faz a ligação entre os usuários e seus perfis de acesso.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_usuario` | `INT` | PK, FK → usuario.id_usuario |
| `id_perfil` | `INT` | PK, FK → perfil_acesso.id_perfil |

### 50. `perfil_permissao` — Administração


Faz a ligação entre cada perfil e as permissões que ele possui.

| Campo | Tipo | Chave / regra |
|---|---|---|
| `id_perfil` | `INT` | PK, FK → perfil_acesso.id_perfil |
| `id_permissao` | `INT` | PK, FK → permissao.id_permissao |

## Relacionamentos principais

`endereco` se relaciona com `paciente` em 1:N. Isso significa que um endereço pode estar ligado a vários pacientes.

`paciente` se relaciona em 1:N com `contato_paciente`, `responsavel_paciente`, `agendamento`, `internacao`, `solicitacao_exame`, `prescricao` e `fatura`.

`funcionario` se relaciona em 1:1 com `medico` ou `enfermeiro`, que guardam informações profissionais específicas. `cargo` e `setor` se relacionam em 1:N com `funcionario`.

`especialidade` se relaciona em 1:N com `medico`. Um médico pode aparecer em vários `agendamento`, `prescricao` e `solicitacao_exame`.

`agendamento` pode ter 0 ou 1 `consulta`. Uma `consulta` pode usar várias salas, e uma sala pode receber várias consultas; essa ligação é feita pela tabela `consulta_sala`.

`paciente` tem um `prontuario` (1:1). O `prontuario` pode ter vários diagnósticos, ligados pela tabela `prontuario_diagnostico`, além de registros de alergias e histórico clínico.

`internacao` pode ter 0 ou 1 `alta_hospitalar` e pode ter várias `evolucao_enfermagem`. Um `quarto` pode ter vários `leito`, e um `setor` pode ter vários quartos e salas de consulta.

`tipo_exame` pode aparecer em várias `solicitacao_exame`. Cada solicitação pode ter um resultado e é ligada a um laboratório pela tabela `exame_laboratorio`.

`prescricao` pode ter vários `item_prescricao`. Um `medicamento` pode aparecer em vários itens de prescrição e em vários registros de estoque; o estoque também registra suas movimentações.

`fatura` pode ter vários `item_fatura` e vários `pagamento`. Um `convenio` pode estar ligado a várias faturas e autorizações.

`hospital` pode ter várias `unidade`, e cada unidade pode ter vários `setor`. Um setor também pode ter vários equipamentos.

`usuario` pode estar ligado a vários `perfil_acesso`, e um perfil pode estar ligado a vários usuários. Essa relação é feita por `usuario_perfil`. Os perfis também podem ter várias permissões, ligadas por `perfil_permissao`.

## Cuidados de integridade e segurança
Campos identificadores devem ser obrigatórios (`NOT NULL`) quando a regra de negócio exigir.
Validar CPF/CNPJ e formatos na aplicação; `CHAR(11)`/`CHAR(14)` não validam dígitos verificadores sozinhos.
Armazenar senhas somente como hash forte com salt (por exemplo, Argon2id ou bcrypt), nunca em texto puro.
Restringir acesso a dados clínicos por função e registrar acessos relevantes; dados de saúde são dados pessoais sensíveis pela LGPD.
Definir regras de exclusão (`RESTRICT`, `CASCADE` ou exclusão lógica) cuidadosamente para preservar histórico médico.
Criar índices nas FKs e em campos usados frequentemente em buscas, após avaliar consultas reais.
Definir `NOT NULL`, `DEFAULT`, `CHECK` e regras de unicidade conforme as regras do hospital.

## Observações para a apresentação
O diagrama contém as 50 tabelas distribuídas em módulos para facilitar a leitura.
PK identifica unicamente cada linha; FK cria a ligação entre tabelas.
Tabelas associativas resolvem relacionamentos muitos-para-muitos.
Esta é uma proposta acadêmica simplificada, não um sistema clínico pronto para produção.