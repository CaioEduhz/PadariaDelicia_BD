## README — Projeto de Banco de Dados | Padaria Delícia II

Este documento reúne o conteúdo das duas etapas do projeto de modelagem do banco de dados.

---

## ENTREGA 1 — Modelo Conceitual (DER)

Entrega 1 — Modelo Conceitual (DER)  
*Modelagem de um sistema de gestão de informações para uma organização de pequeno porte*

| Integrante | RGM | GitHub |
|------------|-----|------------------|
| Caio Eduardo Hernandez  | 47791934  | [CaioEduhz](https://github.com/CaioEduhz) |
| Davi Henrique Santos Nascimento  | 47434899 | [davihenryn](https://github.com/davihenryn2007-cmd) |
| Gustavo Reis   | 47455152 | [GustaDoBronx](https://github.com/GustaDoBronx) |
| Isabela Calisto Oliveira	 | 47645695 | [Isalist0](https://github.com/Isalist0) |
| Maria Eduarda Peres | 47644451 | [maduperesz](https://github.com/maduperesz) |


**## 1. Caracterização da Organização**  

A *Padaria Delícia II Ltda.*, nome fantasia *Padaria Delícia II*, é uma microempresa com fins lucrativos localizada na Rua Impata, nº 765, Vila Ré, São Paulo-SP, CEP 03663-010. Inscrita no CNPJ 13.565.217/0001-90, iniciou suas atividades em 19 de abril de 2011 e encontra-se com situação cadastral ativa. Sua atividade principal é o **comércio varejista de produtos de padaria e confeitaria, com predominância de revenda, enquanto sua atividade secundária compreende a fabricação de produtos de padaria e confeitaria com predominância de produção própria.**

A organização desempenha papel relevante na comunidade local ao comercializar produtos de consumo cotidiano e atender moradores da Vila Ré e de regiões próximas. A equipe é composta por 12 pessoas, sendo 2 sócios e 10 funcionários responsáveis pela operação diária, e a padaria atende em média 200 clientes por dia, além de aproximadamente 50 pedidos de delivery diários. Seu porte é adequado à realização de uma pesquisa acadêmica, pois permite observar diretamente processos como atendimento ao público, vendas, produção, controle de estoque, compras e organização administrativa.

De acordo com o levantamento realizado junto à gestão, a *Padaria Delícia II* enfrenta uma crise operacional evidenciada pelos seguintes problemas centrais: **P1)** ausência de um controle sistemático do fluxo de entrada e saída de clientes; **`P2)** falhas no acompanhamento do consumo individual, permitindo que clientes se retirem sem efetuar o pagamento e gerando prejuízos financeiros; **P3)** dependência de métodos informais de gestão, baseados majoritariamente na comunicação verbal e no uso pontual de fichas de papel; e **P4)** controle de estoque realizado de maneira totalmente manual. Essa situação evidencia a necessidade urgente de um sistema integrado que centralize, automatize e padronize os processos de caixa, clientes e estoque.

A escolha da *Padaria Delícia II* como objeto de estudo justifica-se pela proximidade geográfica e facilidade de contato com os integrantes do grupo, pela sua relevância comunitária, pelo porte apropriado para análise e pela disponibilidade da gestão em contribuir com o projeto. A empresa apresenta problemas de organização que podem ser solucionados ou reduzidos mediante a implementação de ferramentas de gestão mais eficientes. Dessa forma, constitui um caso prático para analisar processos existentes e desenvolver soluções aplicáveis à sua rotina operacional.

A existência da organização pode ser comprovada pelo cadastro empresarial ativo, pelos diretórios comerciais e pelo perfil público da padaria no Instagram. Os cadastros consultados informam como administradores Francisco Almir Pereira e José Aldir das Chagas, além do telefone (11) 2958-2840 e do e-mail [pyoner@ig.com.br](mailto:pyoner@ig.com.br).

Além disso, há um registro anexado abaixo da reunião de levantamento de dados realizada por videoconferência com o sócio da empresa. Embora um dos integrantes do grupo frequente presencialmente o estabelecimento, a reunião foi realizada nesse formato devido à preferência do responsável pela padaria, considerando a disponibilidade de agendas dos envolvidos.

[Print da primeira reunião com um dos sócios da padaria](img/Reuniao.jpg)

[Também, consulte o resumo do levantamento de dados](Levantamento/Levantamento.md)

---

**## 2. Processos de Negócio**  

Para garantir a rastreabilidade entre as necessidades da Padaria Delícia e a modelagem do banco de dados, os processos de negócio foram mapeados e estruturados detalhadamente a seguir.

---
### 2.1. Processos Principais (Vendas e Atendimento)

#### Processo 1: Atendimento no Salão e Balcão
- **Objetivo:** Registrar o consumo presencial de produtos (por quilo ou unidade) e efetuar a cobrança no caixa.
- **Atores:** Cliente, Balconista, Operador de Caixa, Sistema Saurus.
- **Entrada / Gatilho:** Cliente solicita um produto no balcão ou consome no salão.
- **Fluxo Principal:**
  1. O Balconista pesa o produto (se aplicável), embala e informa o valor.
  2. O Balconista anota manualmente o item e valor na **comanda de papel**.
  3. O Cliente recebe a comanda e o produto.
  4. Ao finalizar o consumo/compras, o Cliente se dirige ao Caixa com a comanda de papel.
  5. O Operador de Caixa insere os itens no sistema Saurus e informa o valor total.
  6. O Cliente realiza o pagamento (Dinheiro, PIX ou Cartão).
  7. O Operador de Caixa registra o pagamento, emite o Cupom Fiscal no sistema Saurus e encerra a comanda.
- **Saída:** Cupom/Nota Fiscal emitida, baixa de estoque e registro de venda no caixa.
- **Dados Envolvidos no Processo:**
  - *Comanda (Papel/Sistema):* Número da comanda, data, hora de abertura, local de consumo (balcão ou salão), itens (produto, forma de venda [UN ou KG], quantidade/peso em kg, valor unitário/preço por kg e valor total do item).
  - *Fechamento/Pagamento:* Operador de caixa, valor total da venda, forma de pagamento (dinheiro, PIX, cartão de crédito/débito), valor pago, troco, data/hora de encerramento, número do cupom fiscal emitido pelo Saurus e CPF do cliente na nota (opcional/a validar).
- **Exceções e Regras de Negócio:**
  - *RN16 — Venda de bebida alcoólica:* Exige obrigatoriamente a conferência de documento de identidade caso o cliente aparenta ser menor de 18 anos.
  - *Evasão:* Clientes que saem sem pagar (problema crítico decorrente do controle atual em papel).

#### Processo 2: Gestão de Pedidos via Delivery
- **Objetivo:** Receber, preparar, despachar e confirmar entregas solicitadas à distância.
- **Atores:** Cliente, Operador de Caixa, Equipe de Produção, Motoboy (Terceirizado).
- **Entrada / Gatilho:** Chegada de pedido via WhatsApp da padaria ou plataforma iFood.
- **Fluxo Principal:**
  1. **Recepção:**
     - *WhatsApp:* Caixa atende, consulta/cadastra o cliente e anota o pedido manualmente.
     - *iFood:* Pedido é impresso automaticamente no terminal de vendas.
  2. O Caixa encaminha a ordem de produção/separação para a cozinha/balcão.
  3. A Produção/Balcão separa o pedido e devolve para a expedição.
  4. O Motoboy terceirizado é acionado e retira o pedido com a indicação da taxa e do endereço.
  5. O Motoboy realiza a entrega e coleta a validação (código de verificação no iFood ou pagamento em maquininha).
- **Saída:** Pedido entregue, taxa de entrega apurada e pagamento registrado no caixa.
- **Dados Envolvidos no Processo:**
  - *Pedido:* Canal (WhatsApp ou iFood), número do pedido, data/hora de recebimento, dados do cliente (nome, telefone, endereço completo [logradouro, número, complemento, bairro, CEP] e distância em km).
  - *Itens do Pedido:* Produto, quantidade e observações do item.
  - *Valores e Entrega:* Valor dos itens, taxa de entrega, valor total, forma de pagamento eletrônico, status do pedido (*recebido, em separação, aguardando motoboy, em rota, entregue*), nome/telefone do motoboy, horário de saída, horário de entrega e código de validação/comprovante.
- **Exceções e Regras de Negócio:**
  - *RN12 — Raio e Taxa via WhatsApp:* Limite máximo de entrega de até 5 km com taxa fixa de R$ 4,00.
  - *RN12 — Raio e Taxa via iFood:* Limite máximo de até 10 km com taxa calculada pela plataforma.
  - *Forma de Pagamento:* No delivery, o pagamento deve ser feito exclusivamente por meio eletrônico.

#### Processo 3: Encomendas Especiais (Bolos, Tortas e Salgados)
- **Objetivo:** Registrar pedidos antecipados personalizados garantindo a cobertura dos custos de produção.
- **Atores:** Cliente, Atendente de Caixa, Cozinha/Produção.
- **Entrada / Gatilho:** Solicitação de encomenda presencial ou via WhatsApp com antecedência.
- **Fluxo Principal:**
  1. Atendente registra os itens encomendados, data e horário de retirada.
  2. Atendente cobra **20% de sinal** do valor total da encomenda.
  3. O pedido é agendado no calendário de produção da cozinha.
  4. No dia combinado, a Produção prepara a encomenda.
  5. O Cliente retira o produto mediante o pagamento dos 80% restantes.
- **Saída:** Produto entregue e sinal/saldo quitados.
- **Dados Envolvidos no Processo:**
  - *Encomenda:* Número da encomenda, data do pedido, canal (presencial ou WhatsApp), dados do cliente (nome e telefone).
  - *Itens da Encomenda:* Produto (bolo, torta ou salgado), quantidade, tamanho/peso, sabor/recheio, personalização (ex: mensagem no bolo) e observações.
  - *Agendamento e Financeiro:* Data e horário de retirada, valor total, valor do sinal (20%) com data e forma de pagamento, saldo remanescente (80%) com data e forma de pagamento, e status (*agendada, em produção, pronta, retirada, cancelada, não retirada*).
- **Exceções e Regras de Negócio:**
  - *RN05 — Antecedência Mínima:* Mínimo de 24 horas (1 dia) para a realização do pedido.
  - *Retenção de Sinal:* Se o cliente não retirar a encomenda, o valor de 20% retido cobre os custos de insumos e não é devolvido.
  - *Cancelamento:* Aceito apenas se a produção do item ainda não tiver sido iniciada.

---

### 2.2. Processos de Apoio e Gestão Financeira

#### Processo 4: Controle e Fechamento de Caixa
- **Objetivo:** Garantir a conferência dos valores transacionados e a segurança no encerramento de turnos.
- **Atores:** Operador de Caixa, Sócio / Gerente.
- **Entrada / Gatilho:** Horário de troca de turno (14h e 22h).
- **Fluxo Principal:**
  1. O Operador de Caixa encerra os lançamentos no sistema Saurus.
  2. O sistema gera o relatório com o valor esperado (vendas em dinheiro vs. pagamentos digitais).
  3. O Operador realiza a contagem do dinheiro físico no gaveteiro.
  4. O valor informado é confrontado com o total registrado pelo sistema.
  5. Caso haja divergência, a conferência é auditada e ajustada pelo Sócio.
- **Saída:** Relatório de fechamento de caixa emitido e conferência auditada.
- **Dados Envolvidos no Processo:**
  - *Fechamento:* Data, turno (14h ou 22h), operador responsável, horário de encerramento, valor inicial do caixa (troco), valores esperados pelo sistema discriminados por forma de pagamento (dinheiro, PIX, crédito, débito).
  - *Conferência e Auditoria:* Valor contado em dinheiro físico, cálculo da diferença (esperado x informado), motivo da divergência (se houver), sócio auditor, data/hora da auditoria e status (*conferido, ajustado, pendente*).
  - *Sangria:* Data/hora, valor retirado, motivo e responsável (se houver).
- **Exceções e Regras de Negócio:**
  - *RN07 — Fechamento de Caixa:* Obrigatoriedade de realizar o fechamento duas vezes ao dia (às 14h e às 22h).
  - *Sangria:* Pouco frequente, visto que 80% a 90% das transações ocorrem via pagamentos digitais.

#### Processo 5: Gestão de Compras e Estoque
- **Objetivo:** Manter os níveis de insumos equilibrados para evitar falta de produtos críticos e desperdícios.
- **Atores:** Responsável por Compras (Sócio), Fornecedores.
- **Entrada / Gatilho:** Conferência semanal do nível de estoque praticado.
- **Fluxo Principal:**
  1. O Responsável por Compras analisa o estoque atual e a média de consumo semanal fixada (ex: meta de 5 fardos de refrigerante/semana).
  2. Calcula-se a necessidade de reposição: {Quantidade a comprar} = {Média Semanal} - {Estoque Remanescente}.
  3. O pedido é faturado via boleto bancário com o fornecedor escolhido (a padaria trabalha com mais de 20 fornecedores).
  4. **Recebimento:** No ato da entrega, toda mercadoria é conferida fisicamente antes da estocagem.
- **Saída:** Estoque reabastecido e lançamento das obrigações a pagar no financeiro.
- **Dados Envolvidos no Processo:**
  - *Cadastro do Insumo/Produto:* Código, nome, categoria, unidade de medida, estoque atual, estoque mínimo e média de consumo semanal.
  - *Pedido de Compra:* Fornecedor, data do pedido, data prevista de entrega, itens solicitados (quantidade, valor unitário e valor total do pedido).
  - *Recebimento/Conferência:* Data/hora do recebimento, responsável pela conferência, número da nota fiscal/documento de entrega, quantidade pedida vs. recebida, data de validade, estado da mercadoria e resultado (*aceita* ou *rejeitada* com motivo).
- **Exceções e Regras de Negócio:**
  - *RN14 — Conferência Obrigatória:* Se houver divergência, avarias ou produto vencido no ato da entrega, a carga é obrigatoriamente recusada e devolvida.

#### Processo 6: Pagamento de Fornecedores
- **Objetivo:** Quitar os compromissos financeiros gerados pelas aquisições de insumos, mercadorias de revenda e despesas (água, luz, etc.).
- **Atores:** Sócio / Administrador, Fornecedores.
- **Entrada / Gatilho:** Vencimento dos boletos ou faturas emitidas pelos fornecedores.
- **Fluxo Principal:**
  1. O Sócio confere os boletos bancários relativos às compras de insumos, mercadorias de revenda e despesas (água, luz, etc.).
  2. O Sócio efetua o pagamento bancário na data de vencimento acordada.
  3. O comprovante de pagamento é arquivado e o registro financeiro é atualizado.
- **Saída:** Títulos de fornecedores liquidados e controle de passivos atualizado.
- **Dados Envolvidos no Processo:**
  - *Título/Boleto:* Fornecedor, número do boleto/documento fiscal, compra relacionada, valor da fatura, data de emissão e data de vencimento.
  - *Quitação:* Data do pagamento, valor efetivamente pago, responsável pelo pagamento, comprovante arquivado (número de autenticação bancária/arquivo) e status (*a pagar* ou *pago*).

#### Processo 7: Contas a Pagar (Despesas Operacionais Gerais)
- **Objetivo:** Controlar e realizar o pagamento das contas de consumo e manutenção do estabelecimento.
- **Atores:** Sócio / Administrador.
- **Entrada / Gatilho:** Recebimento de faturas de despesas recorrentes (energia elétrica, água, internet, manutenção, etc.).
- **Fluxo Principal:**
  1. O Sócio recebe e centraliza as faturas de despesas operacionais do mês.
  2. O Sócio realiza os pagamentos devidos dentro dos prazos de vencimento.
  3. Os registros de quitação são arquivados para a gestão financeira interna.
- **Saída:** Custos fixos e operacionais quitados, garantindo a continuidade das operações da padaria.
- **Dados Envolvidos no Processo:**
  - *Despesa Operacional:* Tipo de despesa (energia elétrica, água, internet, etc.), credor/concessionária, mês de competência, valor, data de vencimento, status (*a pagar* ou *paga*), data do pagamento e comprovante arquivado.

---

### 2.3. Processos de Produção e Operação Interna

#### Processo 8: Planejamento e Registro de Produção Diária
- **Objetivo:** Definir e registrar a quantidade de panificação e confeitaria a ser produzida no dia.
- **Atores:** Equipe de Produção (Padeiros/Confeiteiros).
- **Entrada / Gatilho:** Início do expediente da equipe de produção (06h).
- **Fluxo Principal:**
  1. A equipe analisa a quantidade de sobras do dia anterior e a demanda esperada para o turno.
  2. A equipe define a quantidade de fornadas/áreas a serem produzidas.
  3. A produção é realizada e disponibilizada para a área de vendas/balcão.
- **Saída:** Produtos prontos disponibilizados no salão e balcão.
- **Dados Envolvidos no Processo:**
  - *Lote de Produção:* Data, turno, responsável (padeiro ou confeiteiro), produto, quantidade de sobras do dia anterior, quantidade planejada (fornadas), quantidade produzida, data/hora da disponibilização e local de destino (balcão ou salão).
  - *Perdas e Descartes:* Produto descartado, quantidade, motivo da perda, data/hora e responsável pelo registro.
- **Melhoria Necessária com o Sistema:**
  - *RN06 — Registro Prévio:* Passar a registrar formalmente no sistema os lotes produzidos e eventuais perdas/descartes (que atualmente não são contabilizados).

#### Processo 9: Gestão de Turnos e Controle de Acessos
- **Objetivo:** Administrar as escalas dos colaboradores e restringir operações críticas de segurança e governança.
- **Atores:** Funcionários, Sócios.
- **Entrada / Gatilho:** Troca de horários de trabalho e acesso ao sistema.
- **Fluxo Principal:**
  1. O 1º Turno atua das 06h às 14h.
  2. O 2º Turno atua das 14h às 22h.
  3. As permissões no sistema são parametrizadas de acordo com o papel e cargo do usuário.
- **Saída:** Escala operacional executada e operações auditadas.
- **Dados Envolvidos no Processo:**
  - *Colaborador:* Nome completo, CPF, cargo (balconista, operador de caixa, padeiro, confeiteiro, sócio), turno alocado e usuário de acesso.
  - *Auditoria de Operações:* Perfil de acesso, permissões concedidas, log de operações críticas (usuário, operação realizada, data, hora e justificativa).
- **Regras de Negócio:**
  - *Alçada de Acesso:* Permissões para alterar preços, cadastrar produtos, aplicar descontos, cancelar itens ou visualizar relatórios gerenciais são **estritamente exclusivas dos Sócios**.

[Fluxo de Processos](img/Fluxograma.png)

---

**## 3. Requisitos do Sistema**  

***3.1 Requisitos Funcionais***

- **RF01 — Cadastro de produtos.** Código, nome, descrição, categoria, preço de venda, custo, unidade, estoque, estoque mínimo, validade (quando aplicável), status. Deve permitir anexar documentos regulatórios (ver RF25).

- **RF02 — Controle de categorias.** Cadastro, edição, consulta e exclusão de categorias de produtos.

- **RF03 — Registro de vendas.** Produtos, quantidades, preço, descontos, forma de pagamento, valor recebido, troco, com cálculo automático do total.

- **RF04 — Emissão de comprovante.** Nome da padaria, data/hora, número da venda, produtos, valores, forma de pagamento.

- **RF05 — Controle automático de estoque.** Após a conclusão da venda, o sistema deverá permitir a emissão ou visualização de um comprovante contendo: Nome da padaria; Data e horário; Número da venda;	Produtos vendidos;	Quantidades;	Valores;	Descontos;	Forma de pagamento;	Valor total.

- **RF06 — Alerta de estoque mínimo.** Gerar alerta quando um produto atingir o estoque mínimo configurado, com prioridade maior para produtos essenciais (ex.: pão francês) — ver RN10.

- **RF07 — Controle de validade.** Registro de datas de validade, alerta de vencimento próximo e bloqueio de venda de produtos vencidos.

- **RF08 — Controle de produção.** Registro dos produtos produzidos por dia, com atualização automática do estoque.

- **RF09 — Ficha técnica dos produtos.** Composição/ingredientes de cada produto produzido internamente; a ficha técnica também é um dos laudos obrigatórios anexados (RF25). 

- **RF10 — Controle de matéria-prima.** Baixa automática de ingredientes conforme a produção registrada.

- **RF11 — Cadastro de fornecedores.** Razão social, CNPJ/CPF, contato, endereço, produtos fornecidos.

- **RF12 — Registro de compras.** Fornecedor, data, produtos, quantidades, valores, nota fiscal; atualiza o estoque ao confirmar.

- **RF13 — Cadastro de clientes.** Base de dados de clientes (nome, contato, histórico de compras). Distinto do controle de consumo durante o atendimento, que é tratado no RF28.

- **RF14** **— Pedidos antecipados.** Cliente, produtos, data/horário de retirada, status do pedido.

- **RF15** **— Encomendas personalizadas.** Sabor, tamanho, recheio, tema, data de entrega, observações — para bolos, tortas e kits de festa.

- **RF16 — Controle de caixa.** Abertura, fechamento e conferência (total vendido, por forma de pagamento, sangrias, suprimentos, diferença apurada). Prioridade máxima: hoje o fechamento depende quase inteiramente de controle verbal.

- **RF17 — Sangria e suprimento.** Registro de retiradas e entradas de dinheiro no caixa, com valor, horário, responsável e motivo.

- **RF18** **— Controle de funcionários.** Cadastro de funcionários e níveis de acesso. Equipe atual: 2 sócios (cargos de confiança) e 10 funcionários operacionais. 

- **RF19 — Controle de permissões.** Restrição de operações por perfil de usuário \*e por turno de trabalho\* (manhã/noite), refletindo o modelo de acesso já praticado pela padaria.

- **RF20 — Cancelamento e devolução.** Registro de venda original, produto, motivo, responsável e data, com ajuste automático de estoque quando aplicável.

- **RF21 — Relatórios gerenciais.** Prioridade confirmada: vendas por período. Desejável: relatório de desperdício. Complementares: produtos mais/menos vendidos, faturamento, formas de pagamento, estoque, validade, compras, produção, desempenho de funcionários, clientes frequentes. Exportação para Excel/PDF é opcional, não obrigatória.

- **RF22 — Dashboard (escalável).** Painel inicial com totais operacionais do dia (vendas, produto mais vendido, estoque baixo, encomendas, caixa atual) — sem necessidade de indicadores calculados (KPIs).

- **RF23** **— Pesquisa e filtros.** Busca de produtos, clientes, fornecedores, pedidos e vendas por nome, código, categoria, data, status etc. Cadastro de produtos e consulta de preço são as buscas mais frequentes e precisam ser rápidas.

- **RF24** **— Histórico de alterações.** Registro de usuário, ação, data/hora em operações importantes, para segurança e auditoria.

- **RF25** **— Anexação de documentos e laudos regulatórios.** Permitir anexar PDF/imagem a produtos ou ao cadastro da empresa: ficha técnica, laudo de bombeiro, laudo sanitário, alvará de funcionamento, código de defesa do consumidor.

- **RF26** **— Controle de consumo por comanda.** Abrir comanda ao iniciar o atendimento no salão, registrar os itens consumidos e vincular o fechamento ao pagamento antes da liberação do cliente. Ataca diretamente a causa do prejuízo relatado: clientes que saem sem pagar.

- **RF27 — Integração com WhatsApp (Escalável).** Notificações de status de pedido/encomenda e confirmação de delivery via WhatsApp.

***3.2 Requisitos Não Funcionais***

- **RNF01 — Desempenho.** Resposta em até 2 segundos nas operações comuns, especialmente busca de produto e preço.

- **RNF02 — Disponibilidade.** Sistema disponível durante todo o horário de funcionamento: segunda a sexta, 5h30–22h; sábado e domingo, 6h–22h.

- **RNF03 — Segurança.** Autenticação obrigatória para funções administrativas; senhas armazenadas com criptografia/hash.

- **RNF04 — Controle de acesso.** Cada usuário acessa apenas o que seu perfil e turno permitem.

- **RNF05 — Integridade dos dados.** Vendas, estoque e pagamentos não podem ser alterados de forma inconsistente.

- **RNF06 — Backup.** Backups periódicos do banco de dados. (Classificado pela gestão como desejável, não obrigatório.)

- **RNF07 — Recuperação de dados.** Restauração a partir de backup válido em caso de falha.

- **RNF08 — Usabilidade.** Interface simples o suficiente para uma equipe operacional de 10 pessoas usar após treinamento básico.

- **RNF09 — Responsividade.** Interface adaptável a computadores, tablets, notebooks e terminais de atendimento.

- **RNF10 — Compatibilidade.** Funcionamento correto em Chrome, Edge, Firefox e Safari.

- **RNF11 — Escalabilidade.** Suportar o crescimento projetado de faturamento de \~R$ 190 mil/mês para \~R$ 250 mil/mês no ano seguinte, sem reformulação completa.

- **RNF12 — Manutenção.** Código organizado e documentado.

- **RNF13 — Auditoria.** Registro de usuário, data, horário e ação em operações críticas — confirmado como necessário pela gestão.

- **RNF14 — Privacidade.** Proteção de dados pessoais de clientes e funcionários conforme a LGPD. Ainda que a gestão não perceba os dados coletados (nome, telefone) como sensíveis, eles são dados pessoais por definição legal.

- **RNF15 — Confiabilidade.** Evitar perda ou duplicação de registros em vendas, pagamentos, estoque, compras e fechamento de caixa.

- **RNF16 — Recuperação após falhas.** Manter os dados consistentes em caso de falha durante a operação financeira.

- **RNF17 — Tempo de aprendizado.** Novo operador deve aprender venda, consulta e fechamento de caixa em treinamento curto.

- **RNF18 — Escalabilidade de usuários.** Suportar 12 usuários simultâneos (2 sócios \+ 10 funcionários) sem perda de desempenho.

- **RNF19 — Implantação híbrida.** Operação combinando processamento/armazenamento local e sincronização em nuvem, conforme preferência da gestão.

---

**## 4. Regras de Negócio**

- **RN01 — Produto vencido.** Produtos com prazo de validade expirado não podem ser vendidos.

- **RN02 — Estoque insuficiente.** Bloqueia venda acima do disponível em estoque, salvo autorização expressa do administrador/sócio.

- **RN03 — Desconto e alçadas.** Descontos acima de um percentual definido exigem autorização exclusiva dos sócios.

- **RN04 — Cancelamento restrito.** Cancelamentos após o fechamento de caixa exigem autorização do sócio ou administrador.

- **RN05 — Pedido sob encomenda (Sinal e Antecedência).** Exige antecedência mínima de 24 horas, registro de itens, data e horário de retirada, com cobrança prévia de 20% do valor como sinal; caso o cliente não retire o pedido, o sinal é retido para cobrir os custos de insumos.

- **RN06 — Produção diária e perdas.** A produção diária inicia às 06h, e a quantidade produzida deve ser registrada no sistema antes de ser disponibilizada para venda.

- **RN07 — Fechamento e divergência de caixa.** O fechamento de caixa é obrigatório duas vezes ao dia (às 14h e às 22h), comparando o valor esperado pelo sistema com o contado em dinheiro; qualquer diferença exige o registro do motivo e auditoria/ajuste pelo sócio.

- **RN08 — Comanda e controle de atendimento no salão.** Toda comanda de salão ou balcão deve possuir número único, data e hora de abertura, local de consumo, e conter itens detalhados com quantidade ou peso (em kg para produtos vendidos a granel) e valores; o encerramento do atendimento exige o pagamento integral registrado.

- **RN09 — Prioridade de produtos críticos.** Produtos essenciais (ex.: pão francês) geram alertas de estoque mínimo com prioridade de reposição sobre os demais.

- **RN10 — Documentação regulatória.** Todo produto e o próprio estabelecimento devem ter vinculados ficha técnica, laudo de bombeiro, laudo sanitário, alvará de funcionamento e código de defesa do consumidor (exigência legal; a ausência pode gerar multa ou interdição).

- **RN11 — Proteção de dados pessoais (LGPD).** Dados pessoais de clientes e funcionários exigem finalidade definida, controle de acesso e retenção compatível.

- **RN12 — Área de entrega.** Pedidos recebidos diretamente pela padaria (por exemplo, WhatsApp) são atendidos em raio de até 5 km. Pedidos feitos por plataformas de entrega são atendidos em raio de até 10 km. O sistema deve validar o canal de origem e a distância antes de confirmar a entrega. Um mesmo cliente pode informar mais de um endereço de entrega, desde que dentro do raio. Interpretação do grupo: o raio de 5 km vale para os pedidos que não vêm de plataforma; confirmar com a gestão. O método de cálculo da distância e o ponto de referência devem ser definidos na implementação.

- **RN13 — Taxa de entrega.** Para pedidos recebidos diretamente pelo WhatsApp, a taxa de entrega informada é fixa em R$ 4,00. Para pedidos feitos por plataformas como iFood, a taxa é definida pela própria plataforma. O sistema deve registrar a taxa efetivamente aplicada em cada entrega, sem presumir que a taxa da plataforma seja igual à taxa do WhatsApp.

- **RN14 — Formas de pagamento e pagamento misto.** A padaria aceita “todas as formas de pagamento”; para o sistema, as modalidades adotadas são DINHEIRO, PIX, DEBITO, CREDITO e VALE_ALIMENTACAO (domínio D07). O cliente pode dividir o pagamento entre pessoas, e uma mesma venda pode combinar modalidades. Cada modalidade usada é registrada como uma parcela em PAGAMENTO_VENDA, com seu valor, e a soma das parcelas deve cobrir o total da venda após o desconto. O troco só existe quando há parcela em dinheiro. Em delivery só valem modalidades eletrônicas (RN16).

- **RN15 — Conferência de mercadoria.** Toda mercadoria deve ser conferida no ato da entrega, antes de ser armazenada. Se houver divergência, avaria ou outra inconformidade, a padaria não recebe a mercadoria. O controle atual de compras e vencimentos é realizado semanalmente; há uma pessoa especificamente responsável pelas compras.

- **RN16 — Pagamento e confirmação de entrega.**: As entregas são realizadas por terceiros/motoboys. A entrevista informou que a entrega utiliza um código de verificação e que o pagamento do delivery deve ser eletrônico. A conclusão da entrega deve ser registrada somente após a confirmação exigida pelo processo.
  
- **RN17 — Encomendas e sinal.**: A padaria aceita encomendas com solicitação feita com um dia de antecedência e solicita sinal de 20%. O valor do sinal é registrado em ENCOMENDA.VL_SINAL. Caso o cliente não retire a encomenda, o sinal cobre parte dos custos para reduzir prejuízos. A regra de cancelamento informada é limitada: encomendas podem ser canceladas se o produto ainda não estiver pronto.

- **RN18 — Fechamento de caixa.**: O fechamento de caixa ocorre duas vezes ao dia, na troca de turnos às 14h e às 22h. A gestão informou que aproximadamente 80% a 90% dos pagamentos são digitais e que atualmente não há necessidade de sangria. Decisão do grupo: mesmo com a baixa porcentagem de pagamentos em dinheiro, a estrutura de MOVIMENTACAO_CAIXA será criada para receber os registros de sangria e suprimento; a sangria não deve ser descrita como rotina atual da padaria.

- **RN19 — Controle semanal de estoque.**: A reposição é baseada em uma média de consumo semanal: a quantidade comprada repõe o que foi consumido para manter a média habitual. O sistema deve apoiar consulta de consumo e estoque para auxiliar a pessoa responsável pelas compras. A quantidade exata de fornecedores não é conhecida; o entrevistado estimou que sejam mais de 20, com compras distribuídas entre fornecedores.

- **RN20 — Produção, sobras e perdas.** Atualmente não há receita padronizada nem registro sistêmico de produção, sobras ou descartes. A equipe de produção faz uma estimativa no início do expediente e a análise da quantidade a produzir considera as sobras. A ficha técnica e o controle de perdas existentes no modelo devem ser tratados como funcionalidades propostas para o sistema futuro, não como processos já praticados pela padaria.

- **RN21 — Venda por peso.** O balconista pesa o produto, embala e registra o preço na comanda de papel; o cliente paga depois no caixa. O sistema deverá suportar quantidade fracionada/peso e o preço calculado conforme o peso e o valor vigente do produto.

- **RN22 — Cancelamento e devolução.**: Segundo a entrevista, o cancelamento é aceito em casos de encomenda desde que o produto ainda não esteja pronto. Devoluções podem ser aceitas quando não houver indício de má-fé do cliente. Os critérios objetivos, a pessoa que registra a ocorrência e o fluxo de autorização devem ser formalizados pela gestão antes da implantação.
  
- **RN23 — Contas a pagar.**: A padaria paga os fornecedores por boleto e controla os vencimentos semanalmente. Cada vencimento é registrado em CONTA_PAGAR, ligada a um fornecedor. DT_PAGAMENTO fica vazia até o pagamento. A ligação com COMPRA não foi definida e não faz parte desta versão.

- **RN24 — Vínculo com o cupom fiscal.**: A emissão de cupom/nota é feita pelo sistema Saurus. Este sistema só guarda o código emitido em VENDA.CD_CUPOM_FISCAL, preenchido após a emissão. A forma de obter o código (digitação, arquivo ou integração) ainda será definida.

- **RN25 — Proibição de venda de bebidas alcoólicas a menores.**: É proibida a comercialização de bebidas alcoólicas a menores de 18 anos. A maioridade deve ser verificada por documento de identificação sempre que o cliente apresentar aparência jovem, conforme a legislação vigente.
  
---

***4.1 Requisitos e regras prováveis não incluídos (justificativas)***

- **Relatório de consumo e sugestão de compra**: A reposição é semanal com base na média de uso. O sistema apoia essa decisão com relatórios e consultas, sem necessidade de sugestão automática nesta fase.

- **Consulta de comandas abertas**: A necessidade já é atendida por requisitos e regras existentes, utilizando filtros por status da comanda.

- **Integração com o Saurus**: A entrevista confirmou que o Saurus cuida da emissão fiscal e do backup, mas a forma de conexão exata ainda é uma pendência técnica dependente do fornecedor.

- **Consentimento de marketing e retenção**: Os direitos e a finalidade de privacidade já estão cobertos por regras gerais.

- **Comanda aberta no fechamento do caixa**: A situação de comandas que ficam abertas na troca de turnos não foi detalhada pela padaria e pode ser definida futuramente com a gestão.

- **Ajuste manual de estoque**: Como a padaria não registra perdas ou sobras atualmente, o estoque se movimenta apenas por vendas, compras, produção e devoluções. Ajustes manuais ficam para uma fase futura.

- **Cadastro de entregador**: As entregas são feitas por terceiros e confirmadas por código, dispensando uma nova entidade; caso necessário, o nome pode ser incluído nas observações da entrega.

---

**## 5. Dicionário de Dados**  

Confira o [Dicionário de Dados](Dicionario/index.html).

---

**## 6. Modelagem Conceitual (Entidades, Atributos, Relacionamentos)**  

-  **Entidades reconhecidas:**

**CATEGORIA**: utilizada para organizar os produtos em grupos, como pães, confeitaria, bebidas e salgados. 

**PRODUTO:** representa os produtos acabados comercializados pela padaria, sejam de produção própria ou de revenda.  

**MATERIA\_PRIMA:** representa os insumos utilizados na produção, como farinha, ovos e fermento, que possuem controle de estoque próprio.  

**ITEM\_FICHA\_TECNICA:** entidade associativa que relaciona produtos e matérias-primas, registrando a quantidade de cada insumo necessária para produzir determinado produto.  

**DOCUMENTO\_REGULATORIO:** armazena documentos e laudos regulatórios, podendo estar relacionado a um produto específico ou ao estabelecimento.  

**FORNECEDOR:** representa as pessoas ou empresas responsáveis pelo fornecimento de produtos, serviços e matérias-primas.  

**COMPRA:** registra as compras realizadas pela padaria junto aos fornecedores.  

**ITEM\_COMPRA:** detalha os produtos de revenda ou matérias-primas presentes em cada compra.  

**PRODUCAO:** registra os produtos produzidos, a quantidade produzida, a data e o funcionário responsável.  

**CLIENTE:** armazena os dados básicos dos clientes e permite manter seu histórico de compras.  

**FUNCIONARIO:** representa os sócios e funcionários que utilizam ou operam o sistema, incluindo informações de cargo, turno e acesso.  

**COMANDA:** controla o consumo dos clientes durante o atendimento no salão, permitindo vincular os produtos consumidos ao cliente e ao funcionário responsável.  

**ITEM\_COMANDA:** registra cada produto consumido dentro de uma comanda, incluindo quantidade e valor unitário.  

**VENDA:** representa as vendas realizadas no balcão ou por delivery, incluindo informações de cliente, caixa, pagamento e valores.  

**ITEM\_VENDA**: detalha os produtos, quantidades e valores que compõem cada venda.  

**CANCELAMENTO\_DEVOLUCAO:** registra cancelamentos e devoluções relacionados às vendas, incluindo motivo, produto envolvido e funcionário responsável.  

**CAIXA:** representa uma sessão de caixa, desde sua abertura até o fechamento e conferência dos valores.  

**MOVIMENTACAO\_CAIXA:** registra movimentações financeiras realizadas durante uma sessão de caixa, como sangrias e suprimentos.  

**LOG\_AUDITORIA:** registra ações relevantes realizadas pelos usuários do sistema, permitindo rastreabilidade e auditoria.  

**ENCOMENDA:** representa pedidos antecipados ou personalizados de bolos, tortas e salgados, feitos com pelo menos um dia de antecedência. Guarda a data de retirada ou entrega, a situação do pedido, o valor total e o sinal de 20% cobrado na encomenda.  

**ITEM_ENCOMENDA:** detalha cada produto de uma encomenda, com a quantidade e o valor unitário combinado. Permite que uma mesma encomenda tenha vários itens, como um bolo e um kit de salgados.

**PAGAMENTO_VENDA:** Cada ocorrência é uma parcela do pagamento de uma venda, em uma única modalidade. Uma venda paga de uma só forma tem uma ocorrência; uma venda com pagamento dividido ou misto (por exemplo, parte em dinheiro e parte em PIX, ou a conta dividida entre pessoas) tem várias. 

**DELIVERY:** Representa a entrega de uma venda realizada por delivery, armazenando endereço, taxa, saída, conclusão e situação. A entrevista confirmou pedidos por WhatsApp da padaria e por aplicativos como iFood.

**CONTA_PAGAR:** Registra o que a padaria deve a cada fornecedor e quando vence.


-  **Atributos e classificações:**

As principais identificações de cada entidade são:

* **ID\_:** identificador da entidade, utilizado como chave primária ou chave estrangeira quando aplicável.  

* **NM\_:** nome.  

* **CD\_:** código ou informação de identificação.  

* **DT\_:** data ou data e hora.  

* **QT\_:** quantidade.  

* **VL\_:** valor monetário.  

* **TP\_:** tipo, categoria ou status.  

* **IN\_:** indicador booleano.  

* **DS\_:** descrição ou texto livre.  

 
Os principais atributos de cada entidade são:

**CATEGORIA:** ID_CATEGORIA, NM_CATEGORIA.

**PRODUTO:** ID_PRODUTO, CD_PRODUTO, NM_PRODUTO, DS_PRODUTO, VL_PRECO_VENDA, VL_CUSTO, CD_UNIDADE, QT_ESTOQUE, QT_ESTOQUE_MINIMO, DT_VALIDADE, IN_PRODUTO_ESSENCIAL, IN_ATIVO.

**MATERIA_PRIMA:** ID_MATERIA_PRIMA, NM_MATERIA_PRIMA, CD_UNIDADE, QT_ESTOQUE, QT_ESTOQUE_MINIMO.

**ITEM_FICHA_TECNICA (associativa):** QT_QUANTIDADE.

**DOCUMENTO_REGULATORIO:** ID_DOCUMENTO, TP_DOCUMENTO, DS_ARQUIVO, DT_EMISSAO, DT_VALIDADE.

**FORNECEDOR:** ID_FORNECEDOR, NM_FORNECEDOR, CD_CNPJ_CPF, NM_CONTATO, CD_TELEFONE, DS_ENDERECO.

**COMPRA:** ID_COMPRA, DT_COMPRA, CD_NOTA_FISCAL, VL_TOTAL.

**ITEM_COMPRA:** ID_ITEM, QT_QUANTIDADE, VL_UNITARIO.

**CONTA_PAGAR:** ID_CONTA_PAGAR, DS_DESCRICAO, DT_VENCIMENTO, VL_VALOR, TP_STATUS, DT_PAGAMENTO.

**PRODUCAO:** ID_PRODUCAO, DT_PRODUCAO, QT_PRODUZIDA.

**CLIENTE:** ID_CLIENTE, NM_CLIENTE, CD_TELEFONE, DT_CADASTRO.

**FUNCIONARIO:** ID_FUNCIONARIO, NM_FUNCIONARIO, TP_CARGO, TP_TURNO, CD_LOGIN, DS_SENHA_HASH, IN_ATIVO.

**COMANDA:** ID_COMANDA, DT_ABERTURA, DT_FECHAMENTO, TP_STATUS.

**ITEM_COMANDA:** ID_ITEM, QT_QUANTIDADE, VL_UNITARIO.

**VENDA:** ID_VENDA, DT_VENDA, TP_CANAL, VL_TOTAL, VL_DESCONTO, VL_RECEBIDO, VL_TROCO, TP_STATUS, CD_CUPOM_FISCAL.

**ITEM_VENDA:** ID_ITEM, QT_QUANTIDADE, VL_UNITARIO, VL_DESCONTO_ITEM.

**PAGAMENTO_VENDA:** ID_PAGAMENTO, TP_FORMA_PAGAMENTO, VL_PAGO.

**DELIVERY:** ID_DELIVERY, DS_ENDERECO_ENTREGA, VL_TAXA_ENTREGA, DT_SAIDA, DT_ENTREGA, TP_STATUS, DS_OBSERVACAO, TP_CANAL_ORIGEM.

**CANCELAMENTO_DEVOLUCAO:** ID_CANCELAMENTO, DT_CANCELAMENTO, DS_MOTIVO.

**CAIXA:** ID_CAIXA, DT_ABERTURA, DT_FECHAMENTO, VL_ABERTURA, VL_TOTAL_SISTEMA, VL_TOTAL_INFORMADO, VL_DIFERENCA, TP_STATUS.

**MOVIMENTACAO_CAIXA:** ID_MOVIMENTACAO, TP_MOVIMENTACAO, VL_VALOR, DT_HORA, DS_MOTIVO.

**LOG_AUDITORIA:** ID_LOG, NM_TABELA, TP_ACAO, DT_HORA, DS_DETALHE.

**ENCOMENDA:** ID_ENCOMENDA, DT_RETIRADA_ENTREGA, TP_STATUS, VL_TOTAL, VL_SINAL, DS_PERSONALIZACAO.

**ITEM_ENCOMENDA:** ID_ITEM, QT_QUANTIDADE, VL_UNITARIO.

 
- **Relacionamentos pertinentes:** como as entidades se conectam.

**CATEGORIA — PRODUTO:** uma categoria pode agrupar vários produtos, enquanto cada produto pertence a uma categoria (1:N).  

**PRODUTO — ITEM\_FICHA\_TECNICA:**\ um produto pode possuir vários itens em sua ficha técnica (1:N).  

**MATERIA\_PRIMA \- ITEM\_FICHA\_TECNICA:** uma matéria-prima pode participar de várias fichas técnicas (1:N), formando um relacionamento N:M entre PRODUTO e 
MATERIA\_PRIMA.  

**PRODUTO — PRODUCAO:** um produto pode possuir vários registros de produção ao longo do tempo (1:N).  

**FUNCIONARIO — PRODUCAO:** um funcionário pode registrar várias produções .  

**FORNECEDOR — COMPRA:** um fornecedor pode originar várias compras (1:N).  

**COMPRA — ITEM\_COMPRA:** uma compra possui vários itens (1:N).  

**PRODUTO — ITEM\_COMPRA:** um produto de revenda pode aparecer em vários itens de compra (1:N).  

**MATERIA\_PRIMA — ITEM\_COMPRA:** uma matéria-prima pode aparecer em vários itens de compra (1:N).  

**PRODUTO — DOCUMENTO\_REGULATORIO:** um produto pode possuir vários documentos regulatórios vinculados (1:N), sendo esse vínculo opcional.  

**CLIENTE — COMANDA:** um cliente pode possuir várias comandas ao longo do tempo, sendo o vínculo opcional quando o cliente não é identificado (1:N).  

**FUNCIONARIO — COMANDA:** um funcionário pode abrir ou atender várias comandas (1:N).  

**COMANDA — ITEM\_COMANDA:** uma comanda possui vários itens consumidos (1:N).  

**PRODUTO — ITEM\_COMANDA:** um produto pode aparecer em vários itens de diferentes comandas (1:N).  

**FUNCIONARIO — CAIXA:** um funcionário pode abrir e, quando aplicável, fechar várias sessões de caixa (1:N).  

**CAIXA — VENDA:** uma sessão de caixa pode conter várias vendas (1:N).  

**FUNCIONARIO — VENDA:** um funcionário pode realizar várias vendas (1:N).  

**CLIENTE — VENDA:** um cliente pode estar associado a várias vendas, sendo esse relacionamento opcional (1:N).  

**COMANDA — VENDA:** uma comanda fechada pode originar no máximo uma venda de pagamento, sendo o relacionamento opcional (1:1).  

**VENDA — ITEM\_VENDA:** uma venda possui um ou mais itens vendidos (1:N).  

**PRODUTO — ITEM\_VENDA:** um produto pode aparecer em vários itens de venda (1:N).  

**VENDA — CANCELAMENTO\_DEVOLUCAO:** uma venda pode possuir zero ou vários registros de cancelamento ou devolução (1:N).  

**FUNCIONARIO — CANCELAMENTO\_DEVOLUCAO:** um funcionário pode ser responsável pela autorização de vários cancelamentos ou devoluções (1:N).  

**CAIXA — MOVIMENTACAO\_CAIXA:** uma sessão de caixa pode possuir várias movimentações financeiras (1:N).  

**FUNCIONARIO — MOVIMENTACAO\_CAIXA:** um funcionário pode ser responsável por várias movimentações de caixa (1:N).  

**FUNCIONARIO — LOG\_AUDITORIA:** um funcionário pode gerar vários registros de auditoria (1:N).  

**CLIENTE — ENCOMENDA:** um cliente pode realizar várias encomendas (1:N).  

**PRODUTO — ITEM_ENCOMENDA:** um produto pode ser encomendado em vários itens, e cada item refere-se a um único produto (1:N).

**VENDA — PAGAMENTO_VENDA:**  uma venda é paga por uma ou mais modalidades (1:N) / (1:1).

**PRODUTO — CANCELAMENTO_DEVOLUCAO:** a devolução parcial referencia o produto; o cancelamento de venda inteira não (0:N) / (0:1).

**VENDA — DELIVERY:** venda de balcão não tem entrega; venda por delivery gera no máximo uma entrega (0,1) / (1,1).

**FORNECEDOR — CONTA_PAGAR:**  um fornecedor pode ter várias contas a pagar (boletos); toda conta pertence a um fornecedor (0,N) / (1,1).

**ENCOMENDA — ITEM_ENCOMENDA:** uma encomenda contém um ou mais itens, e cada item pertence a uma única encomenda (1:N).
 

- **Restrições e políticas organizacionais aplicadas ao modelo.**

**Produto vencido:** produtos vencidos não podem ser vendidos.  
**Estoque insuficiente:** a venda acima do estoque disponível deve ser bloqueada, salvo autorização do administrador.  
**Desconto:** descontos acima do percentual definido exigem autorização.  
**Cancelamento:** cancelamentos realizados após o fechamento do caixa exigem autorização do sócio ou administrador.  
**Pedido sob encomenda:** deve respeitar a antecedência mínima definida pela padaria.  
**Produção:** a quantidade produzida deve ser registrada antes da disponibilização do produto para venda.  
**Fechamento de caixa:** o valor esperado pelo sistema deve ser comparado ao valor informado pelo funcionário, permitindo identificar diferenças.  
**Comanda em aberto:** o atendimento não pode ser encerrado com uma comanda em aberto sem que exista pagamento registrado.  
**Produtos críticos:** produtos essenciais, como o pão francês, possuem prioridade nos alertas de estoque mínimo.  
**Documentação regulatória:** documentos e laudos exigidos devem poder ser vinculados aos produtos ou ao estabelecimento.  
**Proteção de dados:** os dados pessoais de clientes e funcionários devem possuir controle de acesso e finalidade de uso adequada à LGPD.  
**Controle de acesso:** as operações disponíveis devem respeitar o perfil e o turno do funcionário.  
**Auditoria:** operações relevantes devem gerar registros no LOG\_AUDITORIA, permitindo identificar o usuário, a ação realizada e o momento da operação.

---

**## 7. Diagrama Entidade-Relacionamento (DER)**  

O Diagrama Entidade-Relacionamento (DER), feito pelo software BRModelo (BRMW), foi anexado junto ao repositório, confira [***aqui***](https://app.brmodeloweb.com/publicview/6ae3cf51222486b04fbbc9b4).

---

**## 8. Justificativa Técnica**  
A justificativa técnica do modelo de dados da Padaria Delícia II parte da necessidade de resolver os principais problemas identificados na empresa: falta de controle de clientes, falhas no acompanhamento do consumo, dificuldade no fechamento de caixa e gestão manual do estoque. Por isso, o modelo foi construído de forma proporcional à realidade da padaria, priorizando as entidades e relacionamentos que atendem diretamente aos requisitos funcionais e às regras de negócio levantadas com a gestão.

As entidades PRODUTO e MATERIA_PRIMA foram separadas porque possuem finalidades diferentes. O produto é vendido ao cliente e possui preço, categoria, validade e estoque voltado à comercialização. Já a matéria-prima é utilizada na produção e não possui preço de venda. A separação evita atributos nulos, inconsistências e regras extras para impedir que insumos sejam vendidos como produtos acabados.

A entidade CATEGORIA foi criada separadamente para organizar os produtos de forma padronizada. Um campo de texto livre poderia causar duplicidades e erros de grafia, prejudicando filtros e relatórios. Com uma entidade própria, é possível cadastrar, editar e consultar categorias de maneira consistente.

A separação entre COMANDA e VENDA foi uma das decisões mais importantes do modelo. A comanda representa o consumo em aberto no salão ou balcão, enquanto a venda representa o pagamento efetivamente realizado. Essa divisão evita que consumos não finalizados sejam contabilizados como faturamento e permite controlar melhor o problema de clientes que saem sem pagar. As entidades ITEM_COMANDA e ITEM_VENDA também são separadas porque uma registra o consumo e a outra registra o que foi cobrado.

O CAIXA foi modelado como uma sessão de abertura e fechamento, e não apenas como um atributo da venda. Isso permite comparar o valor esperado pelo sistema com o valor informado pelo funcionário, registrando eventuais diferenças. A MOVIMENTACAO_CAIXA foi criada separadamente para registrar sangrias e suprimentos, que não devem ser confundidos com vendas.

A entidade CANCELAMENTO_DEVOLUCAO foi incluída para registrar motivo, data, responsável e produto envolvido em cancelamentos e devoluções. Um simples campo de “venda cancelada” não seria suficiente, principalmente em casos de devolução parcial, em que apenas parte dos itens precisa ser ajustada no estoque.

A DOCUMENTO_REGULATORIO foi modelada de forma genérica, com um atributo que identifica o tipo de documento. Fichas técnicas, laudos sanitários, laudos do corpo de bombeiros e alvarás possuem estrutura parecida, portanto não seria necessário criar uma entidade diferente para cada tipo. Essa decisão facilita a manutenção do sistema caso novos documentos sejam exigidos.

Funcionários e sócios foram representados na mesma entidade FUNCIONARIO, diferenciados pelos atributos de cargo e turno. Como a padaria possui apenas 12 pessoas, não havia necessidade de criar uma hierarquia mais complexa. O controle de acesso é realizado por perfil e turno, conforme as regras de permissão da empresa.

A entidade LOG_AUDITORIA foi criada para registrar ações importantes realizadas pelos usuários, como alterações de preço, cadastros e cancelamentos. Esse registro permite rastrear quem realizou cada operação, quando ela ocorreu e quais dados foram afetados, atendendo aos requisitos de segurança e auditoria.

A entidade ENCOMENDA foi mantida no modelo, mas com estrutura simplificada, pois alguns detalhes do processo ainda não foram totalmente validados com a gestão. A personalização foi mantida em um campo descritivo, evitando a criação de estruturas complexas sem necessidade comprovada. Essa foi uma das alterações realizadas na modelagem do banco de dados, buscando maior coerência entre o modelo e as informações realmente confirmadas em campo.

Os atributos do modelo utilizam prefixos padronizados, como ID_, NM_, CD_, DT_, QT_, VL_, TP_, IN_ e DS_, facilitando a compreensão do tipo de dado armazenado. Alguns valores calculáveis, como estoque atual, valor total da venda e diferença de caixa, foram armazenados diretamente para melhorar o desempenho das consultas mais frequentes. Essa é uma desnormalização controlada, que exige cuidado na atualização dos dados para preservar a integridade das informações.

O valor unitário foi mantido nas entidades de itens para preservar o preço praticado no momento da venda, compra ou consumo. Se o valor fosse obtido apenas da tabela de produtos, qualquer alteração de preço modificaria o histórico financeiro, prejudicando relatórios e conferências de caixa.

As cardinalidades foram definidas de acordo com a operação real da padaria. Relacionamentos muitos-para-muitos foram resolvidos por meio de entidades associativas, como ITEM_FICHA_TECNICA, ITEM_VENDA, ITEM_COMANDA e ITEM_COMPRA, pois esses relacionamentos possuem atributos próprios. O relacionamento entre COMANDA e VENDA é um para um opcional, pois nem toda venda vem de uma comanda e nem toda comanda já foi finalizada.

A participação do cliente em vendas e comandas é opcional, pois exigir identificação em todos os atendimentos dificultaria a rotina de uma padaria que atende cerca de 200 clientes por dia. Assim, o modelo mantém a possibilidade de histórico de compras sem comprometer a agilidade do atendimento.

Dessa forma, o modelo possui 24 entidades e busca equilibrar completude, simplicidade e aplicabilidade prática. Algumas estruturas comuns em sistemas maiores foram deixadas de fora por não serem essenciais para o porte atual da padaria. As alterações feitas na modelagem do banco de dados tiveram como objetivo alinhar o projeto às informações validadas com a gestão, garantindo um modelo mais coerente, funcional e adequado à realidade da Padaria Delícia II.

***8.1 Critério geral de abstração***

O recorte do modelo foi guiado pela crise operacional levantada em campo controle de caixa,controle de clientes e controle de estoque,e não pela tentativa de representar exaustivamente tudo o que uma padaria faz.Cada entidade do modelo precisa responder a pelo menos um requisito funcional confirmado pela gestão;o que não passou nesse teste foi deliberadamente deixado de fora e registrado na seção 5.20, para que a ausência seja lida como decisão e não como lacuna.Esse critério é o que sustenta as escolhas descritas a seguir: em uma microempresa de 12 pessoas, cada entidade adicional é um formulário a mais que alguém precisa preencher todos os dias,e estrutura não utilizada degrada a qualidade do dado em vez de melhorá-la.

***8.2 Por que essas entidades***

PRODUTO e MATERIA\_PRIMA separados. A alternativa seria uma entidade única (ITEM) com um indicador de tipo,o que reduziria o número de tabelas.Optamos pela separação porque as duas coisas têm ciclos de vida e atributos incompatíveis: produto acabado tem preço de venda, categoria,validade exposta ao consumidor e é objeto de venda; insumo não é vendido,não tem preço de venda e só se movimenta por compra e produção.Unificá-las produziria uma entidade com metade dos atributos sempre nulos e exigiria regras de aplicação para impedir a venda de farinha, restrição que a separação resolve estruturalmente.

CATEGORIA como entidade, não como atributo textual.Um campo texto em PRODUTO seria mais simples,mas o RF02 pede cadastro,edição,consulta e exclusão de categorias como operação própria,e o RF21/RF23 dependem de agrupamento consistente para relatórios e filtros.Texto livre permitiria "Pães", "pao" e "PÃES" convivendo na mesma base e inviabilizaria qualquer relatório por categoria.

COMANDA separada de VENDA. Esta é a decisão central do modelo, porque é ela que ataca o prejuízo relatado (clientes que saem sem pagar). A alternativa seria representar o atendimento como uma venda em status "aberta", economizando duas entidades. Rejeitamos essa opção por três motivos: (i) comanda e venda são eventos de naturezas diferentes uma é consumo em curso, a outra é uma transação financeira concluída,com forma de pagamento,troco e vínculo a uma sessão de caixa; (ii) tratar consumo em aberto como venda contaminaria todo o faturamento e o fechamento de caixa com registros que ainda não são receita; (iii) a RN08 ("não encerrar atendimento com comanda em aberto sem pagamento") só é expressável de forma limpa se existir um objeto "comanda" com status próprio, independente da existência de uma venda. A duplicação aparente entre ITEM\_COMANDA e ITEM\_VENDA é, portanto, intencional: o primeiro registra o que foi consumido, o segundo registra o que foi efetivamente cobrado e a diferença entre os dois é exatamente a informação que a padaria hoje não consegue apurar.

CAIXA como entidade (sessão), não como atributo de VENDA. Registrar apenas "data" e "operador" na venda permitiria somar o faturamento do dia, mas não permitiria o fechamento exigido pelo RF16 e pela RN07. O que a gestão precisa conferir é uma sessão: valor de abertura, vendas do período, sangrias, suprimentos, valor esperado pelo sistema e valor informado pelo funcionário. Isso exige um objeto com início, fim e saldo próprios. MOVIMENTACAO\_CAIXA foi mantida separada porque sangrias e suprimentos (RF17) não são vendas e não podem trafegar pela mesma estrutura sem distorcer o faturamento.

CANCELAMENTO\_DEVOLUCAO como entidade, não como flag em VENDA. Um atributo booleano "cancelada" seria mais econômico, mas destruiria a informação exigida pelo RF20 e pela RN04: motivo, responsável pela autorização, data e, no caso de devolução parcial, qual produto retornou. Um flag também tornaria impossível representar devoluções parciais, que são justamente o caso em que o ajuste de estoque precisa ser proporcional.

DOCUMENTO\_REGULATORIO genérico. A alternativa seria criar uma entidade por tipo de laudo (laudo sanitário, laudo de bombeiro, alvará etc.). Optamos por uma única entidade com o atributo TP\_DOCUMENTO porque todos esses documentos têm exatamente a mesma estrutura tipo, arquivo, emissão, validade — e diferem apenas na classificação. Criar cinco entidades idênticas multiplicaria a manutenção e exigiria alteração do esquema a cada nova exigência regulatória, que é justamente o tipo de mudança externa sobre a qual a empresa não tem controle (RN10).

FUNCIONARIO unificado, com TP\_CARGO e TP\_TURNO. Sócios e funcionários operacionais foram modelados na mesma entidade, sem especialização/generalização.A hierarquia seria defensável em uma organização com muitos perfis distintos, mas aqui há 2 sócios e 10 funcionários operacionais que usam o mesmo sistema e diferem apenas no nível de autorização.Os atributos TP\_CARGO e TP\_TURNO bastam para sustentar o RF19 e o RNF04 (acesso por perfil e por turno), que é o modelo de acesso já praticado informalmente pela padaria.

LOG\_AUDITORIA como entidade transversal.Poderia ser substituída por campos de "última alteração" em cada tabela. Isso, porém, guardaria apenas o estado mais recente, enquanto o RF24 e o RNF13 pedem histórico de operações inclusive de exclusões,que desapareceriam junto com o registro. Uma entidade de log independente preserva o rastro mesmo quando o dado original deixa de existir.

ENCOMENDA tem itens em ITEM_ENCOMENDA porque um pedido antecipado reúne vários produtos. A personalização fica em um único campo de texto livre na encomenda (DS_PERSONALIZACAO), e não em atributos estruturados, porque a gestão ainda não detalhou sabor, tamanho, recheio e tema. Mantivemos a entidade por completude e por ser típica do segmento, mas sem ligá-la a VENDA ou COMANDA,e com a personalização concentrada em um único campo de texto livre (DS\_PERSONALIZACAO) em vez de atributos estruturados para sabor, tamanho,recheio e tema.A razão é de honestidade metodológica: estruturar detalhadamente um requisito que a gestão não validou em campo produziria um modelo aparentemente mais completo, porém apoiado em suposições nossas e não em evidência.

Ausência da entidade associativa ITEM_ENCOMENDA. Embora o mapeamento do processo de encomendas mencione itens no plural, a modelagem conceitual atual não inclui uma entidade associativa para relacionar ENCOMENDA a vários PRODUTOS e registrar, de forma estruturada, a quantidade de cada item. A decisão representa uma simplificação do recorte atual, e não a afirmação de que toda encomenda contenha apenas um produto. A entrevista confirmou o processo de encomenda, a antecedência mínima de um dia, o sinal de 20% e as condições de cancelamento e não retirada; entretanto, a estrutura detalhada dos itens e a forma de registrar cada componente ainda precisariam ser validadas com a gestão. Por isso, a personalização foi mantida no atributo DS\_PERSONALIZACAO, em texto livre, evitando estruturar prematuramente detalhes ainda não consolidados. Caso a necessidade de registrar vários produtos por encomenda seja confirmada, o modelo poderá ser evoluído com a entidade associativa ITEM_ENCOMENDA, vinculada a ENCOMENDA e PRODUTO e contendo o atributo QT_QUANTIDADE. Essa limitação deve ser considerada em uma próxima revisão do modelo.

***8.3 Por que esses atributos***

A nomenclatura por prefixos (ID\_, NM\_, CD\_, DT\_, QT\_, VL\_, TP\_, IN\_, DS\_) foi adotada para que o tipo semântico de cada atributo seja legível no próprio nome,reduzindo ambiguidade na passagem para o modelo lógico e atendendo ao RNF12.

- **Três decisões de atributo merecem defesa explícita:**

Atributos derivados armazenados. QT\_ESTOQUE em PRODUTO e MATERIA\_PRIMA, VL\_TOTAL em VENDA e COMPRA e VL\_DIFERENCA em CAIXA são, em tese, calculáveis a partir das movimentações. Optamos por materializá-los porque o RNF01 exige resposta em até 2 segundos justamente nas operações mais frequentes (consulta de preço e de disponibilidade), e recalcular saldo a cada consulta significaria varrer todo o histórico de vendas, compras, produções e devoluções do produto. É uma desnormalização controlada, cujo custo é a necessidade de manter a consistência transacional exigida pelo RNF05 e RNF15.

VL\_UNITARIO em ITEM\_VENDA, ITEM\_COMANDA e ITEM\_COMPRA. O preço poderia ser sempre lido de PRODUTO.VL\_PRECO\_VENDA, evitando repetição.Rejeitamos isso porque o preço de tabela muda ao longo do tempo e o item de venda precisa preservar o valor praticado no momento da transação sem isso, qualquer reajuste reescreveria retroativamente o faturamento histórico e inviabilizaria o relatório de vendas por período (RF21) e a conferência de caixa.Pelo mesmo motivo, VL\_DESCONTO\_ITEM fica no item, e não apenas no total da venda: a RN03 condiciona autorização a um percentual, que só é verificável no nível em que o desconto foi concedido.

Minimalismo em CLIENTE. A entidade guarda apenas nome, telefone e data de cadastro. A escolha é deliberada e decorre da RN11: cada dado pessoal adicional amplia a superfície de exposição sob a LGPD sem contrapartida operacional. Telefone é suficiente para o histórico de compras (RF13) e para a notificação de delivery (RF27); endereço, CPF e data de nascimento não foram incluídos porque nenhum requisito confirmado os utiliza.

IN\_PRODUTO\_ESSENCIAL foi modelado como indicador booleano em PRODUTO, e não como uma entidade de níveis de prioridade, porque a RN09 estabelece apenas duas classes de comportamento no alerta de estoque mínimo: essencial (ex.: pão francês) e demais. Uma escala de prioridades seria estrutura para uma distinção que a padaria não faz.

***8.4 Por que essas cardinalidades***

1:N como padrão e associativas para N:M. Todos os relacionamentos muitos-para-muitos do domínio foram resolvidos por entidade associativa: ITEM\_FICHA\_TECNICA entre PRODUTO e MATERIA\_PRIMA, ITEM\_VENDA entre VENDA e PRODUTO, ITEM\_COMANDA, ITEM\_COMPRA. Isso não é apenas conformidade com a técnica: em todos esses casos o relacionamento carrega atributos próprios (quantidade, valor unitário, desconto), e atributo de relacionamento só tem onde residir se a associativa existir. A ficha técnica é o exemplo mais claro o que interessa não é que farinha participa de pão, mas quanta farinha.

COMANDA VENDA como 1:1 opcional. A opcionalidade é obrigatória nos dois sentidos e cada lado tem uma razão distinta. Nem toda venda vem de comanda (balcão avulso e os \~50 deliveries diários nascem direto como venda), e nem toda comanda gerou venda ainda (comanda aberta é o estado normal durante o atendimento). Já o limite de 1 do lado da venda representa a regra do negócio: uma comanda fechada corresponde a um único pagamento.Permitir 1:N abriria a porta para pagamentos parciais, prática que não foi observada em campo e cuja modelagem prematura complicaria o fechamento de caixa.

CLIENTE —\> VENDA e CLIENTE —\> COMANDA como participação parcial. Tornar o cliente obrigatório seria tecnicamente mais simples e produziria um histórico completo, mas quebraria a operação: com cerca de 200 atendimentos por dia em uma padaria de bairro, exigir identificação em cada venda de pão tornaria o sistema mais lento que o balcão atual e levaria os funcionários a contorná-lo risco real em uma equipe que hoje trabalha de forma verbal. A cardinalidade mínima zero é, aqui,uma decisão de viabilidade operacional, não uma concessão.

FUNCIONARIO —\> CAIXA com dois vínculos distintos (ID\_FUNCIONARIO\_ABERTURA e ID\_FUNCIONARIO\_FECHAMENTO). A alternativa seria um único relacionamento com um atributo de papel.Mantivemos dois porque a padaria opera em dois turnos (manhã/noite) e é comum que quem abre o caixa não seja quem o fecha; separar os vínculos torna a responsabilidade pela diferença apurada (RN07) explícita no modelo, em vez de dependente de interpretação.

ITEM\_COMPRA com vínculo exclusivo a PRODUTO ou MATERIA\_PRIMA. Cada item de compra referencia um produto de revenda ou um insumo, nunca ambos. A alternativa seria duplicar a entidade (ITEM\_COMPRA\_PRODUTO e ITEM\_COMPRA\_MATERIA\_PRIMA), o que eliminaria a exclusividade condicional ao custo de fragmentar a nota fiscal em duas estruturas paralelas uma compra real do fornecedor frequentemente traz os dois tipos no mesmo documento, e separá-las tornaria o total da compra (VL\_TOTAL) inconferível contra a nota.

DOCUMENTO\_REGULATORIO com ID\_PRODUTO opcional. O vínculo nulo representa o documento do estabelecimento (alvará, laudo de bombeiro), e o vínculo preenchido representa o documento do produto (ficha técnica). A alternativa seria criar uma entidade ESTABELECIMENTO com um único registro apenas para ancorar esses documentos estrutura desproporcional para uma empresa de uma só unidade.

VENDA — CANCELAMENTO\_DEVOLUCAO como 1:N opcional, e não 1:1, porque uma mesma venda pode sofrer devoluções parciais sucessivas de produtos diferentes; e opcional porque a esmagadora maioria das vendas nunca gera cancelamento.

***8.5 Proporcionalidade ao porte da organização***

Algumas estruturas usuais em sistemas de varejo foram conscientemente não modeladas: catálogo fornecedor × produto, entidade de ajuste manual de estoque e desperdício, e indicadores calculados do dashboard (RF22), que são derivações de consulta e não dados a persistir.Em todos os casos o raciocínio foi o mesmo o ganho informacional não compensa o custo de alimentação manual em uma operação de 12 pessoas, e o histórico já existente (ITEM\_COMPRA, por exemplo) responde à necessidade prática. O modelo tem 24 entidades porque esse é o número necessário para cobrir os requisitos confirmados; uma versão com 30 entidades seria mais impressionante no papel e menos utilizável no balcão.

***8.6 Limitações assumidas***

Registramos duas limitações conhecidas do recorte atual. A validade está modelada como atributo de PRODUTO (DT\_VALIDADE), o que pressupõe uma validade vigente por produto e não permite rastrear lotes distintos do mesmo item suficiente para o bloqueio exigido pela RN01 no contexto de uma padaria com giro diário, mas insuficiente caso a empresa passe a trabalhar com estoques de validade longa. E a baixa de matéria-prima pela produção usa a quantidade prevista na ficha técnica, não a quantidade efetivamente consumida, o que não captura perdas de produção decisão coerente com a despriorização do relatório de desperdício pela gestão, e revisitável na mesma estrutura já usada em MOVIMENTACAO\_CAIXA.

---

**## 9. Uso de Inteligência Artificial**  

Se o grupo usou alguma ferramenta de IA (ChatGPT, Claude, Gemini, Perplexity etc.) em qualquer parte do trabalho, registre **para cada uso relevante**:

| Item | O que registrar |
| :--- | :--- |
| **Ferramenta e etapa** | Claude foi usado para a escritura da Justificativa Técnica. |
| **Motivação** | Para retomada da justificativa técnica com alteração das entidades e etc. |
| **Prompt(s) utilizados** | Enviado o README completo do projeto (caracterização da organização, processos de negócio, requisitos, regras de negócio, dicionário de dados, modelagem conceitual, DER, justificativa técnica e registros anteriores de uso de IA) com o seguinte pedido: “Com base nesse README inteiro, faça uma justificativa técnica trazendo todos os porquês das decisões tomadas considerando as entidades (só dando adendo que foi alterado algumas coisas dentro do projeto de modelagem de banco de dados), detalhados responsivamente, e me ofereça 2 opções de texto, somente sem colocar em docx ou algo do tipo.” Em seguida, foi solicitada a elaboração do registro “prompt utilizado” para a seção de uso de IA. |
| **Resposta recebida** | Foram recebidas duas opções de texto para a Seção 8 (Justificativa Técnica). A Opção 1 foi organizada por elemento do modelo: critério de abstração, entidades, atributos, cardinalidades, proporcionalidade ao porte e limitações. A Opção 2 foi organizada por problema: partiu dos problemas levantados em campo (clientes saindo sem pagar, caixa conferido verbalmente e estoque manual) e mostrou qual decisão de modelagem responde a cada um. As duas confrontaram cada escolha com a alternativa descartada. Exemplos: COMANDA separada de VENDA, para não misturar consumo em aberto com receita; PRODUTO e MATERIA_PRIMA separados, para evitar atributos nulos; CLIENTE com participação parcial e dados mínimos, por viabilidade operacional e LGPD; VL_UNITARIO nos itens, para preservar o preço histórico; atributos derivados armazenados, como desnormalização controlada (RNF01). A IA também apontou inconsistências no próprio README: contagem de entidades (19 declaradas, 24 listadas), numeração divergente de regras de negócio (RN10, RN11, RN12, RN16, RN19), referências quebradas (como a menção à “seção 5.20”) e o RF06 citando a RN10 em vez da RN09. |
| **Fontes consultadas e verificadas** | Nenhuma fonte externa. A IA usou apenas o README completo enviado pelo grupo (processos, requisitos, regras de negócio, dicionário de dados, modelagem conceitual e DER). O grupo conferiu o texto com o DER (seção 7), o dicionário de dados (seção 5), os números da entrevista (200 clientes/dia, 50 deliveries, 12 usuários) e as referências a RF e RN, verificando se cada requisito ou regra citado existia e correspondia à decisão descrita. |
| **Trechos rejeitados ou corrigidos** | [Ajustar conforme o que o grupo fez.] Como o modelo de banco de dados foi alterado depois da versão do README usada como base, foram revisadas as partes que descreviam entidades, atributos e relacionamentos que mudaram, e os trechos marcados como pendentes de ajuste foram atualizados. Foram corrigidas as inconsistências de numeração de RF e RN apontadas pela própria IA. Foram descartadas as justificativas que não correspondiam ao raciocínio real do grupo, pois a IA tende a tratar toda escolha como deliberada, mesmo quando foi feita por conveniência. A lista de limitações assumidas (seção 8.6) foi validada pelo grupo. |
| **Justificativa da escolha final** | [Ajustar conforme a opção escolhida.] Foi mantida a [Opção 1 / Opção 2 / combinação das duas], por [seguir a mesma estrutura das seções 5 e 6, facilitando a conferência / ligar cada decisão aos problemas da padaria, deixando a defesa mais clara]. A versão final foi adaptada ao modelo atualizado. As decisões de modelagem foram tomadas pelo grupo durante o levantamento com a gestão e a construção do DER. A IA foi usada para organizar e explicitar por escrito uma argumentação que já existia. |
| **Reflexão crítica** | A IA tende a apresentar toda decisão como intencional e bem fundamentada, o que gera um texto convincente, mas nem sempre fiel ao proseso real do grupo. Por isso cada justificativa foi revisada. Como o modelo mudou depois da versão enviada, havia o risco de a IA descrever estruturas já ultrapassadas, o que foi tratado com a revisão manual contra o DER atualizado. A qualidade do resultado dependeu do material fornecido: por receber o README completo, a IA não inventou entidades nem regras inexistentes, e ainda identificou erros de numeração que o grupo não tinha percebido. Não foram identificados vieses relevantes, mas o texto não substitui a validação do grupo, que é quem responde pelas decisões de modelagem. |

| Item | O que registrar |
| :--- | :--- |
| **Ferramenta e etapa** | Claude foi usado para a escritura da introdução. |
| **Motivação** | Para mapeamento de informações relevantes e para a revisão de dados faltantes. |
| **Prompt(s) utilizados** | Olá, Claude! Faça uma introdução com base nesse texto, no arquivo da entrevista e nas informações dos links que eu te mandar. Me fale as informações faltantes para uma introdução completa:<br>• **Nome e natureza da organização:** qual organização real o grupo escolheu (com acesso garantido para pesquisa de campo) — pode ser uma empresa (livraria, lanchonete, pet shop), uma ONG, uma associação comunitária ou outra instituição.<br>• **Contexto e porte:** com ou sem fins lucrativos; tamanho da operação; número de pessoas envolvidas (funcionários, voluntários, membros, fiéis); volume de atividades (vendas, atendimentos, doações, rituais, eventos).<br>• **Problemas e necessidades identificados:** qual é a "crise operacional" — o que está desorganizado hoje (planilhas soltas, papel, falta de controle de estoque/doações/cadastros, etc.)?<br>• **Justificativa da escolha:** por que essa organização foi escolhida e por que ela é um bom caso para o projeto?<br>• **Evidências da organização:** comprove que a organização existe e que o grupo teve acesso a ela — ex.: fotos do local/da visita, link da organização no Google (Google Maps/Google Meu Negócio, site, rede social), endereço completo e forma de contato (telefone, e-mail, responsável pela organização).<br>Links:<br>https://wanderboat.ai/restaurants/brazil/regi%C3%A3o-imediata-de-s%C3%A3o-paulo/padaria-del%C3%ADcia/pFyXZmTJSiejlYw5WReEkg<br>https://www.solutudo.com.br/empresas/sp/s-paulo/panificadoras-e-padarias/padaria-delicia-ii-ltda-17960017<br>https://cnpj.biz/13565217000190<br>https://cnpja.com/office/13565217000190<br>https://www.instagram.com/delicia2padaria?igsi=cDIxa3dnMmhtMHly<br>E documento do levantamento de dados com o sócio da empresa. |
| **Resposta recebida** | A Padaria Delícia II Ltda., nome fantasia Padaria Delícia II, é uma microempresa com fins lucrativos localizada na Rua Impata, nº 765, Vila Ré, São Paulo-SP, CEP 03663-010. Inscrita no CNPJ 13.565.217/0001-90, iniciou suas atividades em 19 de abril de 2011 e encontra-se com situação cadastral ativa. Sua atividade principal é o comércio varejista de produtos de padaria e confeitaria, com predominância de revenda, enquanto sua atividade secundária compreende a fabricação de produtos de padaria e confeitaria com predominância de produção própria. A organização desempenha papel relevante na comunidade local ao comercializar produtos de consumo cotidiano e atender moradores da Vila Ré e de regiões próximas. A equipe é composta por 12 pessoas, sendo 2 sócios e 10 funcionários responsáveis pela operação diária, e a padaria atende em média 200 clientes por dia, além de aproximadamente 50 pedidos de delivery diários. Seu porte é adequado à realização de uma pesquisa acadêmica, pois permite observar diretamente processos como atendimento ao público, vendas, produção, controle de estoque, compras e organização administrativa. De acordo com o levantamento realizado junto à gestão, os principais problemas identificados na Padaria Delícia II estão relacionados ao controle de caixa e ao controle de clientes. A empresa não realiza um controle sistemático de entrada e saída de clientes, e o acompanhamento do consumo apresenta falhas, havendo casos em que clientes se retiram sem efetuar o pagamento, o que gera prejuízo financeiro. Esse controle é feito majoritariamente de forma verbal, com uso pontual de fichas de papel, e o controle de estoque também é realizado de maneira totalmente manual. Essa situação caracteriza a crise operacional da empresa e evidencia a necessidade de um sistema que centralize e padronize o controle de caixa, de clientes e de estoque. A empresa apresenta problemas de organização que podem ser solucionados ou reduzidos mediante a implementação de ferramentas de gestão mais eficientes. Dessa forma, constitui um caso prático para analisar processos existentes e desenvolver soluções aplicáveis à sua rotina operacional. A existência da organização pode ser comprovada pelo cadastro empresarial ativo, pelos diretórios comerciais e pelo perfil público da padaria no Instagram. Os cadastros consultados informam como administradores Francisco Almir Pereira e José Aldir das Chagas, além do telefone (11) 2958-2840 e do e-mail pyoner@ig.com.br. |
| **Fontes consultadas e verificadas** | A IA citou somente as fontes fornecidas. Isso se mostra no exemplo: “De acordo com o levantamento realizado junto à gestão”. |
| **Trechos rejeitados ou corrigidos** | Foi adicionada uma justificativa em relação à escolha da empresa e ao local da entrevista, a qual estava faltando para ficar completa e correta ao modelo condizente. Além de serem listados os problemas de forma mais explícita. |
| **Justificativa da escolha final** | Foi mantida a resposta sugerida, somente adicionando informações. A forma escrita foi bem avaliada e compreendida pelo grupo, não havendo alterações do que foi entregue pela IA. |
| **Reflexão crítica** | Não houve vieses, foram fornecidos os materiais necessários para a atividade desejada. |

| Item | O que registrar |
|------|------------------|
| **Ferramenta e etapa** | Gemini foi usado para revisão de requisitos e regras de negócio. |  
| **Motivação** | Havia uma grande quantidade de requisitos, o grupo pensou na possibilidade de existir algum que poderia ser excluído ou editado. |  
| **Prompt(s) utilizados** | Complemente, adicione ou exclua os requisitos funcionais e não funcionais de acordo com tudo que te mandei e com o arquivo texto.txt. |  
| **Resposta recebida** | O que é redundante e pode sair: * **RN13 (Acesso por turno)** e **RN14 (Horário de funcionamento contínuo)** não são realmente regras de negócio no sentido de modelagem — são só repetições do que já está no RF21/RNF04 (acesso por turno) e no RNF02 e na Introdução (horário de funcionamento). Manter os dois só em Section 3 é mais limpo; aqui eles só ocupam espaço sem adicionar nada nas restrições organizacionais.  * A nota "(era RN11 na versão anterior)" é resíduo de edição, não é conteúdo do trabalho.   * A seção final "O que mudou em relação à lista original" também não corresponde a nenhuma seção pedida pelo professor — é só o changelog da conversa. Isso deixa a 4.2 só com as duas restrições que são genuinamente legais/estatutárias (documentação regulatória e LGPD), que são exemplos muito mais fortes.
| **Fontes consultadas e verificadas** | RF01: Cadastro de produtos; RF02: Controle de categorias; RF03: Registro de vendas; RF04: Múltiplas formas de pagamento; RF05: Emissão de comprovantes; RF06: Controle automático de estoque; RF07: Alerta de estoque mínimo; RF08: Controle de validade; RF09: Controle de produção; RF10: Ficha técnica dos produtos; RF11: Controle de matéria-prima; RF12: Cadastro de fornecedores; RF13: Registro de compras; RF14: Cadastro de clientes; RF15: Programa de fidelidade; RF16: Pedidos antecipados; RF17: Encomendas personalizadas; RF18: Controle de caixa; RF19: Registro de sangrias e suprimentos; RF20: Cadastro de funcionários; RF21: Controle de permissões por perfil e turno; RF22: Cancelamentos e devoluções; RF23: Relatórios gerenciais; RF24: Dashboard operacional; RF25: Pesquisa e filtros; RF26: Histórico de alterações; RF27: Anexação de documentos e laudos regulatórios; RF28: Controle de consumo por comanda; RF29: Integração com WhatsApp. RNF01: Desempenho; RNF02: Disponibilidade; RNF03: Segurança; RNF04: Controle de acesso; RNF05: Integridade dos dados; RNF06: Backup; RNF07: Recuperação de dados; RNF08: Usabilidade; RNF09: Responsividade; RNF10: Compatibilidade com navegadores; RNF11: Escalabilidade do sistema; RNF12: Manutenibilidade; RNF13: Auditoria; RNF14: Privacidade e conformidade com a LGPD; RNF15: Confiabilidade; RNF16: Recuperação após falhas; RNF17: Facilidade de aprendizado; RNF18: Suporte a usuários simultâneos; RNF19: Implantação híbrida. RN01: Proibição da venda de produtos vencidos; RN02: Bloqueio de vendas acima do estoque disponível, salvo autorização; RN03: Autorização para descontos acima do limite estabelecido; RN04: Autorização para cancelamentos após o fechamento do caixa; RN05: Antecedência mínima para encomendas; RN06: Registro da produção antes da disponibilização para venda; RN07: Conferência entre o caixa esperado e o valor informado; RN08: Liberação de pontos de fidelidade após confirmação do pagamento; RN09: Proibição de encerramento de atendimento com comanda em aberto sem pagamento; RN10: Prioridade de alertas para produtos essenciais com estoque baixo; RN11: Manutenção da documentação regulatória exigida por lei; RN12: Proteção dos dados pessoais conforme a LGPD. |  
| **Trechos rejeitados ou corrigidos** | Adicionado: RF27, RF28, RF29, RN10, RN11, RN12, RN15 e RN16 com mais detalhamento. |  
| **Justificativa da escolha final** | Mantiveram-se partes do que foi retornado. Mas houve a necessidade de adicionar mais informações pelos levantamentos que foram feitos em grupo com o sócio do estabelecimento|  
| **Reflexão crítica** | A IA foi limitada pelo material fornecido. Ou seja, executou perfeitamente o comando, sem vieses ou alucinações. |

| Item | O que registrar |
|------|------------------| 
| **Ferramenta e etapa** | Claude foi usado para organização do dicionário de dados |  
| **Motivação** | Foi recorrido à essa IA pois o Claude tem proficiência com dados extensos e complexos, algo característico de um dicionário de dados |  
| **Prompt(s) utilizados** | Desenvolva um dicionário de dados, se baseando nos conteúdos enviados nos documentos anexados (arquivos em PDF com os todos os requisitos e com o modelo de dicionário de dados a ser desenvolvido.) |  
| **Resposta recebida** | Exato conteúdo no tópico 5 do presente documento. |  
| **Fontes consultadas e verificadas** | Documentação de modelo de dicionário de dados enviada pelo professor e documentação com informações relevantes da empresa |  
| **Trechos rejeitados ou corrigidos** | Foi retirado o requisito de fidelidade por não fazer sentido com o cenário de uma padaria e foi reformulado o dicionário para que não incluísse o requisito removido. Também a formatação foi reformulada, solicitando que ele entregasse um arquivo HTML com as informações do dicionário. |  
| **Justificativa da escolha final** | É coerente com a modelagem conceitual desenvolvida, além de coincidir com as informações nas outras etapas do trabalho, como o DER e os requisitos. |  
| **Reflexão crítica** | Não foi identificado nenhum erro ou viés algorítmico claro nas respostas fornecidas pela inteligência artificial. |

| Item | O que registrar |
|------|------------------|
| **Ferramenta e etapa** | Gemini foi usado para organização e ordenamento de processos. |  
| **Motivação** | Otimização de tempo e maior organização. |  
| **Prompt(s) utilizados** | Com base na entrevista e todos os dados e processos fornecidos, faça um mapeamento de processos na pretensão da criação de um fluxograma. |  
| **Resposta recebida** | Cadastro e controle de clientes: hoje não há controle sistemático de entrada, saída e consumo dos clientes atendidos no salão, o que abre brecha para clientes saírem sem pagar; Controle de caixa: fechamento e conferência de caixa dependem quase inteiramente de controle verbal, com uso pontual de fichas de papel; Controle de estoque: movimentação de produtos é registrada de forma totalmente manual, sem sistema de apoio; Produção e compras (fluxo de mercadoria): entrada de insumos, produção diária e disponibilização dos produtos para venda; Vendas, atendimento e entregas: atendimento no balcão e pedidos por delivery (cerca de 50 por dia), com integração desejada ao WhatsApp. |  
| **Fontes consultadas e verificadas** | Documentação da entrevista |  
| **Trechos rejeitados ou corrigidos** | Foi mais detalhado cada processo e adicionado partes de contas à pagar e pagamento aos fornecedores. |  
| **Justificativa da escolha final** | É coerente ao que foi pedido, e foi bem revisado pelo grupo. |  
| **Reflexão crítica** | Não foi identificado nenhum erro ou viés algorítmico claro nas respostas fornecidas pela inteligência artificial. |

| Item | O que registrar |
|------|------------------| 
| **Ferramenta e etapa** | MyMapAi foi usado para fluxograma. |  
| **Motivação** | Construir visual em BPMN. |  
| **Prompt(s) utilizados** | Com base na entrevista e todos os dados e processos fornecidos, poderia criar um visual em BPMN? |  
| **Resposta recebida** | O exato fluxograma, mas com baixa resolução. |  
| **Fontes consultadas e verificadas** | Os principais processos da empresa, citado na seção 2\. |  
| **Trechos rejeitados ou corrigidos** | Usado ChatGpt na tentativa de melhoria de resolução. |  
| **Justificativa da escolha final** | É coerente ao que foi pedido, e foi bem revisado pelo grupo. |  
| **Reflexão crítica** | Não foi identificado nenhum erro ou viés algorítmico claro nas respostas fornecidas pela inteligência artificial. |  

| Item | O que registrar |
|------|------------------| 
| **Ferramenta e etapa** | Claude (Anthropic) foi usado na redação da Seção 8 (Justificativa Técnica), com base no modelo conceitual das seções 5 e 6.|
| **Motivação** | A Seção 8 exige defender cada decisão de modelagem contra alternativas. O grupo já havia tomado essas decisões, mas a argumentação não estava registrada. A IA foi usada para explicitá-la por escrito.|
| **Prompt(s) utilizados** | Pedido de justificativa técnica das decisões de abstração e modelagem (entidades, atributos, relacionamentos e cardinalidades), acompanhado do arquivo da modelagem conceitual completo.|
| **Resposta recebida** | Justificativa em seis blocos (critério de abstração, entidades, atributos, cardinalidades, proporcionalidade e limitações), sempre confrontando a escolha com a alternativa descartada. Exemplos: \- PRODUTO e MATERIA\_PRIMA separados, para evitar atributos nulos; \- COMANDA separada de VENDA, para não contaminar o faturamento; \- Participação parcial de CLIENTE em VENDA, por viabilidade operacional; \- VL\_UNITARIO no item, para preservar o preço histórico; \- Atributos derivados como desnormalização controlada (RNF01). Uma segunda resposta condensou o texto à metade.|
| **Fontes consultadas e verificadas** | Nenhuma fonte externa. O grupo conferiu o texto com o dicionário de dados (seção 5), o DER (seção 7) e os números da entrevista (200 clientes/dia, 50 deliveries, 12 usuários, 24 entidades).|
| **Trechos rejeitados ou corrigidos** | Foram mantidas apenas as justificativas que correspondiam ao raciocínio real do grupo, pois a IA às vezes tratava como deliberadas escolhas feitas por conveniência. As limitações da seção 8.6 foram validadas pelo grupo.|
| **Justificativa da escolha final** | Foi mantida a versão condensada, por ser proporcional ao peso da seção e preservar a comparação com as alternativas. As decisões de modelagem foram do grupo, não da IA.|
| **Reflexão crítica** | A IA tende a apresentar toda decisão como intencional e bem fundamentada, o que gera texto convincente, mas nem sempre fiel ao processo real. Por isso, o grupo revisou cada justificativa. Não houve alucinação, pois o modelo completo foi fornecido, e não foram identificados vieses.|

| Item | O que registrar |
|------|------------------|
| **Ferramenta e etapa** | Claude foi usado na etapa de escrita da seção 6 (Modelagem Conceitual).|
| **Motivação** | Como o modelo ficou com muitas entidades (24 no total) e vários relacionamentos, ficava difícil escrever tudo em texto corrido sem esquecer nada ou repetir informação. Usamos a IA para ajudar a organizar esse conteúdo em tópicos (entidades, atributos, relacionamentos e restrições), já que ela consegue lidar melhor com uma quantidade grande de dados do que a gente escrevendo manualmente. |
| **Prompt(s) utilizados** | Poderia listar todos com base no arquivo enviado? Preciso separar em entidades, atributos, relacionamentos (com cardinalidade) e restrições, seguindo o que já está no dicionário de dados que te mandei antes. |
| **Resposta recebida** | A modelagem conceitual foi desenvolvida a partir dos principais processos e requisitos identificados na Padaria Delícia II, buscando representar as informações necessárias para o controle de produtos, estoque, produção, compras, vendas, clientes e operações de caixa. **6.1 Entidades reconhecidas:** foram listadas as 29 entidades do modelo, cada uma com uma frase curta explicando sua função (por exemplo, a entidade COMANDA existe justamente para resolver o problema real da padaria, que é cliente saindo sem pagar; e CAIXA representa cada sessão, da abertura até a conferência final). **6.2 Atributos e classificações:** a resposta manteve a mesma convenção de prefixos que já estava no dicionário de dados (ID_ para chave primária, NM_ para nome, CD_ para código, DT_ para data, QT_ para quantidade, VL_ para valor monetário, TP_ para tipo/categoria e DS_ para descrição). Os atributos foram detalhados para as entidades principais — CATEGORIA, PRODUTO, MATERIA_PRIMA, CLIENTE, FUNCIONARIO, VENDA e CAIXA — e para as demais entidades foi indicado que os atributos completos estão no Dicionário de Dados Conceitual, para não duplicar informação. **6.3 Relacionamentos pertinentes:** foram listados 21 relacionamentos com suas cardinalidades. O ponto que achamos mais importante de entender (e não só copiar) foi o relacionamento N:M entre PRODUTO e MATERIA_PRIMA, que precisou ser resolvido com uma entidade associativa (ITEM_FICHA_TECNICA), porque um relacionamento N:M não pode ser implementado diretamente em um banco relacional. Também aparecem os vínculos opcionais, como CLIENTE — COMANDA e CLIENTE — VENDA (o cliente pode não se identificar) e o relacionamento 1:1 opcional entre COMANDA e VENDA, que representa o momento em que uma comanda aberta no salão se transforma em uma venda fechada. **6.4 Restrições e políticas organizacionais:** foram reunidas as regras de negócio que o modelo precisa respeitar, como não deixar vender produto vencido, bloquear venda acima do estoque, exigir autorização para desconto alto ou para cancelamento depois do caixa fechado, dar prioridade nos alertas de estoque mínimo para produtos essenciais (tipo o pão francês), controlar o acesso por perfil/turno de funcionário, manter log de auditoria nas operações mais sensíveis e seguir a LGPD no tratamento dos dados de cliente e funcionário. |
| **Fontes consultadas e verificadas** | A IA usou apenas o que eu mandei: o dicionário de dados, o levantamento de requisitos e as informações que vieram da entrevista com a dono/gestão da padaria. Não foi citada nenhuma fonte externa. |
| **Justificativa da escolha final** | Decidimos manter o texto porque ele bateu com o que já tínhamos definido no dicionário de dados e no DER, e porque a explicação de cada relacionamento ajudou a gente a confirmar que o modelo fazia sentido com a realidade da padaria (principalmente a parte de comanda/venda, que foi a que mais discutimos como grupo). |
| **Reflexão crítica** | O principal ponto identificado foi a tendência da IA a apresentar todas as entidades e relacionamentos com o mesmo grau de certeza, sem distinguir o que já estava consolidado no dicionário de dados do que exigiria validação adicional, por isso o grupo revisou cada cardinalidade e atributo, conferindo se correspondiam de fato ao levantamento feito com a gestão da padaria. A qualidade da resposta dependeu totalmente do material fornecido: como o dicionário de dados e os requisitos já estavam completos, não houve alucinação de entidades ou regras inexistentes, a IA apenas organizou o que o grupo já havia definido. Não foram identificados vieses ou generalizações incorretas sobre o segmento de padarias. |

---

## Entrega 2 — Modelo Lógico, Implementação SQL e Apresentação Final

## 1\. Relatório Técnico Final

### 1.1 Revisão do Modelo Conceitual 

| Apontamento da correção da E1 | O que mudou no trabalho | Por quê |
| ----- | ----- | ----- |
| Chaves estrangeiras em quase todas as entidades (COMPRA, ITEM\_\*, PRODUTO, VENDA, COMANDA, CAIXA, PRODUCAO, CANCELAMENTO\_DEVOLUCAO, MOVIMENTACAO\_CAIXA, LOG\_AUDITORIA, ENCOMENDA, DOCUMENTO\_REGULATORIO), com "Chave estrangeira (FK)" na legenda. ✅ | Todas as FKs saíram das entidades (ex.: ID\_CATEGORIA em PRODUTO, ID\_CAIXA, ID\_FUNCIONARIO, ID\_CLIENTE e ID\_COMANDA em VENDA). Os vínculos aparecem só nos losangos, com (mín, máx) nos dois lados. O item "Chave estrangeira (FK)" foi retirado da legenda, que ficou só com chave primária, atributo e cardinalidades. | FK é decisão do modelo lógico (seção 2). No conceitual, o vínculo é o relacionamento. |
| Tópico de decisão do grupo. | Tratamento das entidades associativas e de itens (ITEM\_VENDA, ITEM\_COMANDA, ITEM\_COMPRA, ITEM\_ENCOMENDA e ITEM\_FICHA\_TECNICA)**.**  | A entidade ITEM\_FICHA\_TECNICA atua diretamente como uma associativa entre PRODUTO e MATERIA\_PRIMA com o atributo QT\_QUANTIDADE. Já ITEM\_VENDA, ITEM\_COMANDA, ITEM\_COMPRA e ITEM\_ENCOMENDA utilizam um identificador próprio (ID\_ITEM) ligado às entidades principais para registar quantidades e valores.  |
| FKs compondo o identificador das associativas (ITEM\_VENDA, ITEM\_COMANDA, ITEM\_COMPRA, ITEM\_FICHA\_TECNICA). ✅ | ITEM\_FICHA\_TECNICA virou entidade associativa de fato: um retângulo envolvendo o losango "compõe" entre PRODUTO e MATERIA\_PRIMA, com QT\_QUANTIDADE como atributo próprio e cardinalidade (0,n) nos dois lados. ITEM\_VENDA, ITEM\_COMANDA e ITEM\_COMPRA ficam só com ID\_ITEM como identificador, sem FK, e se ligam à entidade pai por relacionamento (1,1)–(1,n). | O identificador composto só nasce na conversão lógica (PK composta). Atributos que dependem de dois lados (quantidade, valor) pertencem a uma associativa. |
| "na prática, um modelo lógico desenhado em notação de Chen". ✅ | Refeito do zero no brModelo: retângulo para entidade, losango para relacionamento, círculo vazio para atributo e círculo preenchido para identificador. | A entrega exige a notação do brModelo. |
| ENTREGA (delivery) ausente, sugestão. ✅ | Criada DELIVERY (ID\_DELIVERY, DS\_ENDERECO\_ENTREGA, VL\_TAXA\_ENTREGA, DT\_SAIDA, DT\_ENTREGA, TP\_STATUS, DS\_OBSERVACAO, TP\_CANAL\_ORIGEM), ligada a VENDA por "possui" (VENDA 0,1 – DELIVERY 1,1). | São cerca de 50 deliveries por dia. Venda de balcão não tem entrega, e cada venda de delivery gera uma entrega. A entidade guarda a taxa (R\$ 4,00 no WhatsApp, definida pela plataforma no iFood) e o status. |
| Origem do pedido de delivery. ✅ | TP\_CANAL\_ORIGEM (WhatsApp ou aplicativo) ficou só em DELIVERY. VENDA mantém TP\_CANAL (balcão ou delivery). | A origem só existe quando há entrega, e manter nos dois lugares duplicaria o dado. A taxa e o raio (5 km ou 10 km) dependem dessa origem. |
| Delivery, compras, produção e fornecedores com uma frase cada; contas a pagar ausente. ✅ | O README passou a ter 9 processos detalhados, com objetivo, atores, gatilho, fluxo, saída e regras. Cada um tem entidade no DER. Foi criada CONTA\_PAGAR (ID\_CONTA\_PAGAR, DS\_DESCRICAO, DT\_VENCIMENTO, VL\_VALOR, TP\_STATUS, DT\_PAGAMENTO). | Processo sem entidade no DER não é resolvido pelo banco. O processo "ausente" agora tem onde guardar boletos e despesas. |
| Vínculo de CONTA\_PAGAR. ✅ | CONTA\_PAGAR se liga a FORNECEDOR pelo relacionamento "gera" (FORNECEDOR 0,n – CONTA\_PAGAR 1,1). | Os processos 6 e 7 pagam boletos de fornecedores e contas como energia e água, que também têm um emissor. ⏳ Se a conta de compra tiver que ser rastreada, o vínculo pode passar para COMPRA. Defina e justifique no README. |
| Venda no balcão e turnos/acesso (parcial). ✅ | O processo 1 descreve balcão e salão, incluindo venda por peso e comanda de papel. O processo 9 descreve os turnos (6h–14h com 7 pessoas e 14h–22h com 5\) e a alçada dos sócios. Apoio no DER: FUNCIONARIO (TP\_CARGO, TP\_TURNO). | A entrevista de 09/10/2026 confirmou o fluxo do balconista e que as decisões privilegiadas são só dos sócios. |
| Ausentes área e taxa de entrega, formas de pagamento, cupom fiscal, conferência de mercadoria e bebida alcoólica a menores. ✅ | Incluídas no README (delivery, pagamento, fiscal, conferência de insumos, álcool). No DER, DELIVERY e PAGAMENTO\_VENDA sustentam as duas primeiras, e VENDA ganhou CD\_CUPOM\_FISCAL para a regra fiscal. | A entrevista confirmou raio de 5 km no WhatsApp e 10 km por plataforma, taxa de R\$ 4,00, "todas as formas" de pagamento e conferência com recusa na entrega. |
| Cupom/nota fiscal. ✅ | VENDA ganhou CD\_CUPOM\_FISCAL, o número do cupom ou nota emitido no Saurus. O Saurus continua emitindo o documento, e o banco só guarda a referência. | Toda venda concluída deve gerar documento fiscal. Sem esse campo, não há como conferir quais vendas têm cupom (consulta de auditoria). A entrevista confirmou que a padaria já usa o Saurus. |
| Forma de pagamento. ✅ | Criada PAGAMENTO\_VENDA (ID\_PAGAMENTO, TP\_FORMA\_PAGAMENTO, VL\_PAGO), ligada a VENDA (VENDA 1,n – PAGAMENTO 1,1). TP\_FORMA\_PAGAMENTO saiu de VENDA. | A padaria aceita dividir a conta entre pessoas e usar várias formas (perg. 3). Com um atributo na venda isso era impossível. |
| Encomenda com sinal de 20% sem atributo; entidade marcada como não confirmada em campo. ✅ | ENCOMENDA ganhou VL\_SINAL. O contorno tracejado de "entidade não confirmada em campo" e o item correspondente da legenda foram retirados. | A entrevista de 09/10/2026 confirmou que a padaria aceita encomendas, com um dia de antecedência e 20% de sinal, retido se o cliente não retirar. |
| Cardinalidades mínimas ausentes (só 1:N e "opcional"). ✅ | Todos os relacionamentos têm (mín, máx) nos dois lados, no DER e no dicionário. | Permite derivar NOT NULL e a participação obrigatória na conversão lógica. |
| SGBD e mecanismo indefinidos ("MySQL 8 ou PostgreSQL 15"), sem collation e sem versão com data. ✅ | Definidos: MySQL 8.0, InnoDB, utf8mb4 e utf8mb4\_0900\_ai\_ci. Versão 2.2 de 19/10/2026, com histórico de versões. | O script SQL (seção 3\) precisa rodar no MySQL. |
| Log de acesso do SGBD, papéis por operação, LGPD sem base legal e retenção. ✅ | Incluídos: seção de registro de acesso ao SGBD, matriz de acesso por operação e tabela LGPD (finalidade, base legal, retenção, acesso). LOG\_AUDITORIA continua como entidade da aplicação. | A entrevista informou que só sócios decidem operações privilegiadas e que nome e telefone são guardados indefinidamente. |


Na segunda reunião não houve captura de tela (print) durante a videoconferência, então a validação é demonstrada através dos seguintes comprovantes:
[Agendamento via WhatsApp](img/Agendamento_Whatsapp.jpeg).
[Histórico da Chamada no Teams](img/Historico_Teams.jpeg).

### 1.2 Conversão do Modelo Conceitual para o Modelo Lógico

- **Entidades → Tabelas:** *como cada entidade virou tabela.*

Cada entidade do modelo conceitual virou uma tabela com o mesmo nome, totalizando 24 tabelas. O identificador de cada entidade virou a chave primária (ID\_..., PK INT), e os relacionamentos viraram chaves estrangeiras (FK). As tabelas se dividem em quatro grupos:

1-Cadastros, sem FK: CATEGORIA, MATERIA\_PRIMA, FORNECEDOR, CLIENTE e FUNCIONARIO.

2- Entidades principais, com FK para os cadastros: PRODUTO, COMPRA, CONTA\_PAGAR, COMANDA, CAIXA, VENDA, ENCOMENDA, DELIVERY e DOCUMENTO\_REGULATORIO.

3- Registros de eventos, com FK para o funcionário responsável: PRODUCAO, MOVIMENTACAO\_CAIXA, CANCELAMENTO\_DEVOLUCAO e LOG\_AUDITORIA.

4- Tabelas associativas: ITEM\_VENDA, ITEM\_COMANDA, ITEM\_COMPRA, ITEM\_FICHA\_TECNICA e ITEM\_ENCOMENDA, que ligam entidades e guardam dados do vínculo.

- **Atributos → Colunas:** *conversão correta dos atributos.*

Cada atributo virou uma coluna com tipo de dado definido: INT para identificadores, VARCHAR para textos e códigos, FLOAT para valores e quantidades, DATE e DATETIME para datas, e BOOLEAN para indicadores. Os nomes seguem prefixos padronizados: ID\_ (identificador), NM\_ (nome), DS\_ (descrição), CD\_ (código), VL\_ (valor), QT\_ (quantidade), DT\_ (data), TP\_ (tipo ou situação) e IN\_ (indicador). O prefixo já indica o tipo do dado: todo ID\_ é INT, todo DT\_ é DATE ou DATETIME, todo IN\_ é BOOLEAN, e todo VL\_ e QT\_ é FLOAT.

- **Relacionamentos → Chaves:** *como os relacionamentos foram traduzidos.*

Os relacionamentos 1:N viraram uma FK na tabela do lado “N”, apontando para a PK do lado “1”. Exemplos: PRODUTO.ID\_CATEGORIA, COMPRA.ID\_FORNECEDOR e ITEM\_VENDA.ID\_VENDA. Os relacionamentos 1:1 opcionais também viraram FK: VENDA.ID\_COMANDA e DELIVERY.ID\_VENDA.

Os relacionamentos N:N foram resolvidos por tabelas associativas, que recebem uma FK para cada lado e guardam os dados do vínculo:

1- ITEM\_FICHA\_TECNICA liga PRODUTO e MATERIA\_PRIMA. A PK é composta pelas duas FKs e o atributo próprio é QT\_QUANTIDADE.

2- ITEM\_VENDA liga VENDA e PRODUTO (quantidade, valor unitário, desconto).

3- ITEM\_COMANDA liga COMANDA e PRODUTO (quantidade, valor unitário).

4- ITEM\_COMPRA liga COMPRA a PRODUTO ou MATERIA\_PRIMA (quantidade, valor unitário).

5- ITEM\_ENCOMENDA liga ENCOMENDA a PRODUTO (registrando a quantidade e o valor unitário de cada produto solicitado pelo cliente em uma encomenda personalizada).

- **Cardinalidades:** *como cada cardinalidade (1:1, 1:N, N:N) foi tratada (inclusive tabelas associativas, se houver).*

*Cada relacionamento do modelo conceitual foi traduzido por uma chave estrangeira (FK) que referencia a chave primária (PK) da tabela relacionada. Nos relacionamentos 1:N, a FK fica na tabela do lado “N” (por exemplo, PRODUTO.ID\_CATEGORIA e ITEM\_VENDA.ID\_VENDA). Quando a participação é opcional, a FK aceita nulo (VENDA.ID\_CLIENTE, VENDA.ID\_COMANDA).*

*Quando há dois relacionamentos entre as mesmas entidades, foram criadas duas FKs: FUNCIONARIO e CAIXA têm “abre” e “fecha”, que geraram ID\_FUNCIONARIO\_ABERTURA e ID\_FUNCIONARIO\_FECHAMENTO. Nas tabelas associativas, a PK é composta pelas FKs dos dois lados (ITEM\_FICHA\_TECNICA: ID\_PRODUTO \+ ID\_MATERIA\_PRIMA). Em ITEM\_COMPRA, as FKs para PRODUTO e MATERIA\_PRIMA são alternativas: cada item se refere a uma ou a outra. Em DOCUMENTO\_REGULATORIO, ID\_PRODUTO é opcional: vazio para documentos do estabelecimento (alvará, laudo) e preenchido para documentos de produto.*

### 1.3 Definição de Chaves Primárias e Estrangeiras

**CATEGORIA**

* PK: ID\_CATEGORIA  
* FK: Nenhuma

**PRODUTO**

* PK: ID\_PRODUTO  
* FK: ID\_CATEGORIA

**MATERIA\_PRIMA**

* PK: ID\_MATERIA\_PRIMA  
* FK: Nenhuma

**ITEM\_FICHA\_TECNICA**

* PK: ID\_PRODUTO, ID\_MATERIA\_PRIMA (Composta)  
* FK: ID\_PRODUTO, ID\_MATERIA\_PRIMA

**DOCUMENTO\_REGULATORIO**

* PK: ID\_DOCUMENTO  
* FK: ID\_PRODUTO


**FORNECEDOR**

* PK: ID\_FORNECEDOR  
* FK: Nenhuma

**COMPRA**

* PK: ID\_COMPRA  
* FK: ID\_FORNECEDOR

**ITEM\_COMPRA**

* PK: ID\_ITEM  
* FK: ID\_COMPRA, ID\_PRODUTO, ID\_MATERIA\_PRIMA 

**PRODUCAO**

* PK: ID\_PRODUCAO  
* FK: ID\_PRODUTO, ID\_FUNCIONARIO

**CLIENTE**

* PK: ID\_CLIENTE  
* FK: Nenhuma

**FUNCIONARIO**

* PK: ID\_FUNCIONARIO  
* FK: Nenhuma  
  **COMANDA**  
* PK: ID\_COMANDA  
* FK: ID\_CLIENTE, ID\_FUNCIONARIO

**ITEM\_COMANDA**

* PK: ID\_ITEM  
* FK: ID\_COMANDA, ID\_PRODUTO 

**VENDA**

* PK: ID\_VENDA  
* FK: ID\_CAIXA, ID\_FUNCIONARIO, ID\_CLIENTE, ID\_COMANDA

**ITEM\_VENDA**

* PK: ID\_ITEM  
* FK: ID\_VENDA, ID\_PRODUTO 

**PAGAMENTO\_VENDA**

* PK: ID\_PAGAMENTO  
* FK: ID\_VENDA

**CANCELAMENTO\_DEVOLUCAO**

* PK: ID\_CANCELAMENTO  
* FK: ID\_VENDA, ID\_PRODUTO, ID\_FUNCIONARIO

**CAIXA**

* PK: ID\_CAIXA  
* FK: ID\_FUNCIONARIO\_ABERTURA, ID\_FUNCIONARIO\_FECHAMENTO

**MOVIMENTACAO\_CAIXA**

* PK: ID\_MOVIMENTACAO  
* FK: ID\_CAIXA, ID\_FUNCIONARIO

**LOG\_AUDITORIA**

* PK: ID\_LOG  
* FK: ID\_FUNCIONARIO


**ENCOMENDA**

* PK: ID\_ENCOMENDA  
* FK: ID\_CLIENTE, ID\_PRODUTO


**ITEM\_ENCOMENDA**

* PK: ID\_ITEM   
* FK: ID\_ENCOMENDA, ID\_PRODUTO, ID\_ENCOMENDA

**DELIVERY**

* PK: ID\_DELIVERY  
* FK: ID\_VENDA

**CONTA\_PAGAR**

* PK: ID\_CONTA\_PAGAR  
* FK: ID\_FORNECEDOR


### 1.4 Processo de Normalização (1FN, 2FN e 3FN)

*(vale 7,5%)*

**\- 1FN:** A entidade *ITEM\_VENDA* permite registrar vários produtos em uma venda, utilizando atributos como *QT\_QUANTIDADE, VL\_UNITARIO* e *VL\_DESCONTO\_ITEM.*  
**Justificativa:** Necessária para eliminar grupos repetitivos e atributos multivalorados, garantindo que cada informação seja armazenada de forma individual e organizada.

**\- 2FN:** Garantia de que todos os atributos dependam da chave primária completa. Por exemplo, os dados descritivos do produto *(NM\_PRODUTO, DS\_PRODUTO e CD\_PRODUTO)* ficam na entidade PRODUTO, separados dos dados específicos dos itens vendidos.  
**Justificativa:** Necessária para evitar que atributos dependam apenas de parte de uma chave primária composta, garantindo que cada informação dependa da chave completa.

**\- 3FN:** Separação de informações que pertencem a entidades diferentes. Por exemplo, os dados do fornecedor *(NM\_FORNECEDOR, CD\_CNPJ\_CPF e CD\_TELEFONE)* ficam na entidade FORNECEDOR, separadamente da entidade COMPRA.  
**Justificativa:** Necessária para eliminar dependências transitivas, evitando informações duplicadas e garantindo que cada atributo dependa diretamente da chave primária da sua entidade.

**RESULTADO:** As três formas ajudam para um banco de dados mais organizado, consistente e com menos redundância.

### 1.5 Justificativas Técnicas das Decisões

## **1\. Introdução**

O modelo conceitual representa um estabelecimento de alimentação com produção própria, venda em balcão, atendimento por comanda, encomendas, delivery, controle de caixa, compras e contas a pagar. Ele possui 24 entidades, organizadas em seis blocos lógicos:

* **Catálogo e produção:** PRODUTO, CATEGORIA, DOCUMENTO\_REGULATORIO, MATERIA\_PRIMA, ITEM\_FICHA\_TECNICA, PRODUCAO.  
* **Suprimentos:** FORNECEDOR, COMPRA, ITEM\_COMPRA, CONTA\_PAGAR.  
* **Vendas:** VENDA, ITEM\_VENDA, PAGAMENTO\_VENDA, CANCELAMENTO\_DEVOLUCAO, CLIENTE.  
* **Atendimento:** COMANDA, ITEM\_COMANDA, ENCOMENDA, ITEM\_ENCOMENDA, DELIVERY.  
* **Caixa:** CAIXA, MOVIMENTACAO\_CAIXA.  
* **Governança:** FUNCIONARIO, LOG\_AUDITORIA.


Os relacionamentos são representados na notação de cardinalidade do BRModelo. Na passagem para o modelo lógico, cada cardinalidade determina o posicionamento das chaves estrangeiras (FK) e a sua obrigatoriedade (NOT NULL ou nula).

## **2\. Justificativa das Chaves Primárias**

### **2.1 Escolha de chaves substitutas (*surrogate keys*)**

Todas as entidades utilizam identificadores numéricos próprios (ID\_PRODUTO, ID\_VENDA, ID\_CAIXA, etc.), gerados automaticamente pelo SGBD. A escolha é fundamentada em três pilares:

1. **Imutabilidade:** Chaves naturais (como códigos de produtos, CNPJ/CPF, números de notas fiscais ou logins) podem sofrer alterações ou correções cadastrais. Utilizar chaves substitutas evita a propagação em cascata de atualizações indesejadas nas tabelas filhas.  
     
2. **Desempenho e Simplicidade:** Tipos numéricos inteiros otimizam a indexação e a performance de junções (*joins*), o que é crítico em tabelas de alto volume como ITEM\_VENDA, PAGAMENTO\_VENDA e LOG\_AUDITORIA.  
     
3. **Padronização:** Uniformiza o comportamento das chaves estrangeiras em todo o projeto, simplificando o código da camada de persistência.  
   

### **2.2 Chaves naturais preservadas como chaves alternativas**

As chaves naturais foram mantidas por meio de restrições de unicidade (UNIQUE), garantindo a integridade negocial sem acoplá-la à identificação física das tuplas:

| Entidade | Chave Alternativa (UNIQUE) | Justificativa de Negócio |
| :---- | :---- | :---- |
| **PRODUTO** | CD\_PRODUTO | Identificador rápido utilizado no balcão, PDV e etiquetas.  |
| **FORNECEDOR** | CD\_CNPJ\_CPF | Identificação unívoca da pessoa física ou jurídica.  |
| **COMPRA** | (ID\_FORNECEDOR, CD\_NOTA\_FISCAL) | A numeração da nota fiscal é única apenas por emitente.  |
| **FUNCIONARIO** | CD\_LOGIN | Requerido para a autenticação segura no sistema.  |
| **VENDA** | CD\_CUPOM\_FISCAL | Assegura a unicidade fiscal quando preenchido.  |

### **2.3 Entidades dependentes (Itens e Associações)**

As tabelas de itens (ITEM\_COMPRA, ITEM\_VENDA, ITEM\_COMANDA, ITEM\_ENCOMENDA e ITEM\_FICHA\_TECNICA) estruturam-se da seguinte forma:

1. ITEM\_FICHA\_TECNICA liga PRODUTO a MATERIA\_PRIMA (com o atributo próprio QT\_QUANTIDADE), implementando a relação N:N da receita.  
     
2. ITEM\_VENDA liga VENDA e PRODUTO (armazenando quantidade, valor unitário e desconto).  
     
3. ITEM\_COMANDA liga COMANDA e PRODUTO (registrando quantidade e valor unitário).  
     
4. ITEM\_COMPRA liga COMPRA a PRODUTO ou MATERIA\_PRIMA (quantidade e valor unitário).  
     
5. ITEM\_ENCOMENDA liga ENCOMENDA a PRODUTO (armazenando a quantidade e o valor unitário de cada item personalizado solicitado pelo cliente).  
   

A adoção de um identificador próprio (ID\_ITEM) associado a chaves estrangeiras em vez de chaves primárias compostas puras em tabelas transacionais de itens permite que o mesmo produto figure em linhas distintas (ex: diferentes faixas de desconto ou observações), além de facilitar referências futuras em estornos e logs. No caso de ITEM\_FICHA\_TECNICA, a garantia de unicidade de insumos por receita é assegurada pela restrição UNIQUE (ID\_PRODUTO, ID\_MATERIA\_PRIMA).

## **3\. Justificativa das Chaves Estrangeiras e dos Relacionamentos**

A regra geral aplicada estabelece que, nos relacionamentos 1:N, a chave estrangeira reside no lado "N", sendo NOT NULL quando a participação for obrigatória (cardinalidade mínima 1\) e opcional quando a mínima for 0\. Nos relacionamentos 1:1 opcionais, a FK recebe obrigatoriamente a restrição UNIQUE.

### **3.1 Catálogo e Produção**

* PRODUTO **→** CATEGORIA (ID\_CATEGORIA, cardinalidade (1,1) e (0,n)): Todo produto pertence obrigatoriamente a uma categoria.  
    
* DOCUMENTO\_REGULATORIO **→** PRODUTO (ID\_PRODUTO, (0,n) e (0,1)): Um produto pode possuir múltiplos documentos de conformidade (alvarás, laudos), e cada documento vincula-se a no máximo um produto.  
    
* PRODUCAO **→** PRODUTO **/** FUNCIONARIO ((1,1) e (0,n)): Cada lote produzido refere-se a um único produto e é registrado por um funcionário responsável, viabilizando o controle de estoque e auditoria.


### **3.2 Suprimentos**

* COMPRA **→** FORNECEDOR (ID\_FORNECEDOR, (1,1)): Toda nota fiscal de entrada possui um fornecedor vinculado.  
    
* ITEM\_COMPRA utiliza um arco exclusivo via restrição CHECK para garantir que cada linha aponte estritamente para uma MATERIA\_PRIMA ou para um PRODUTO (evitando a duplicação de estruturas de tabelas).  
    
* CONTA\_PAGAR **→** FORNECEDOR (ID\_FORNECEDOR, (1,1)): As obrigações financeiras da padaria são direcionadas aos fornecedores cadastrados.


### **3.3 Vendas**

* VENDA **→** FUNCIONARIO **/** CAIXA ((1,1)): Toda venda exige um operador logado e um caixa aberto, viabilizando o confronto de valores entre o sistema e o fechamento físico.  
    
* VENDA **→** CLIENTE (ID\_CLIENTE, (0,1)): O cliente é opcional para permitir vendas rápidas de balcão anônimas.  
    
* VENDA **↔** COMANDA (1:1 opcional): A comanda gera no máximo uma venda ao ser encerrada, e a venda pode ou não ter partido de uma comanda.  
    
* DELIVERY **→** VENDA (ID\_VENDA, 1:1 obrigatório no lado do delivery): A entrega domiciliar depende da existência de uma venda consolidada.  
    
* PAGAMENTO\_VENDA **→** VENDA (1:N): Permite o suporte a pagamentos mistos (múltiplas formas de pagamento para uma única venda).


### **3.4 Atendimento**

* COMANDA **→** FUNCIONARIO **/** CLIENTE: Controla o atendente responsável e o cliente opcional na mesa/comanda.  
    
* ENCOMENDA **→** CLIENTE (ID\_CLIENTE, (1,1)): Diferente do balcão, a encomenda exige identificação prévia do cliente para formalizar prazos, sinal financeiro e retirada.  
    
* ITEM\_ENCOMENDA **→** ENCOMENDA **/** PRODUTO: Associa os produtos específicos encomendados às respectivas quantidades e preços unitários.


### **3.5 Caixa e Governança**

* CAIXA **→** FUNCIONARIO: Possui duas FKs distintas (ID\_FUNC\_ABERTURA e ID\_FUNC\_FECHAMENTO) para auditar de forma independente quem abriu e quem fechou o caixa.  
    
* MOVIMENTACAO\_CAIXA **→** CAIXA **/** FUNCIONARIO: Garante a rastreabilidade de sangrias e suprimentos de numerário.


**4\. Justificativa das Restrições**

**4.1 Obrigatoriedade (`NOT NULL`)**

* **RN01, RN04, RN06, RN08, RN10 e RN23:** Asseguram que transações, produções, comandas, entregas, contas a pagar e documentos fiscais possuam rastreabilidade obrigatória a funcionários, caixas, clientes ou fornecedores conforme as premissas do negócio.

**4.2 Unicidade (`UNIQUE` e `CHECK`)**

* **RN08 (comanda com número único) e RN24:** Garantem a unicidade de cadastros (`CD_PRODUTO`, `CD_LOGIN`, `CD_CNPJ_CPF`, `CD_CUPOM_FISCAL`), o funcionamento do arco exclusivo de itens de compra (`CHECK`) e a exclusividade de insumos na ficha técnica (`UNIQUE (ID_PRODUTO, ID_MATERIA_PRIMA)`).

**4.3 Domínios e Consistência de Valores (`CHECK`)**

* **RN01 (validade), RN02 (bloqueio de estoque), RN05 e RN17 (encomenda e sinal de 20%), RN09 (produtos essenciais/estoque mínimo):** Validam que preços, custos e estoques não assumam valores negativos, além de padronizar os domínios textuais de status (caixa, comanda, delivery, encomendas) e indicadores lógicos booleanos (`IN_ATIVO`, `IN_PRODUTO_ESSENCIAL`).  
* **RN07 e RN18 (fechamento de caixa em turnos), RN13 e RN14 (taxas de entrega e pagamento misto), RN21 (venda por peso/quantidade fracionada):** Validam a coerência cronológica entre datas, o cálculo automático de diferenças de caixa (`VL_DIFERENCA`), o controle de troco, o limite do sinal da encomenda e a obrigatoriedade de motivos em cancelamentos.


## **5\. Decisões Arquiteturais Gerais**

1. **Normalização (3FN):** O modelo atende à Terceira Forma Normal, eliminando redundâncias transitivas e garantindo atomicidade nos atributos.  
     
2. **Redundância Controlada:** O armazenamento histórico de VL\_UNITARIO em tabelas de itens protege o sistema contra alterações futuras de preços de tabela.  
     
3. **Inativação Lógica:** O uso de IN\_ATIVO em PRODUTO e FUNCIONARIO preserva a integridade de registros históricos em vendas e auditorias sem recorrer à exclusão física.  
     
4. **Trilha de Auditoria:** A entidade LOG\_AUDITORIA centraliza o registro de alterações críticas de forma genérica e segura.

   

### 1.6 Proposta de Arquitetura Analítica para BI e IA

*(vale 7,5%)*

# **1.6 Proposta de Arquitetura Analítica para BI e IA**

Padaria Delícia II Ltda. · Base: modelo lógico (BRMW, 24 tabelas) e requisitos RF/RN das seções anteriores.

**Premissas das consultas.** Dialeto PostgreSQL (para MySQL, troque ::date por DATE(), CURRENT\_DATE \- 30 por CURDATE() \- INTERVAL 30 DAY e FILTER (WHERE …) por SUM(CASE …)). Valores de domínio assumidos: VENDA.TP\_STATUS \= CONCLUIDA/CANCELADA; TP\_CANAL \= BALCAO/WHATSAPP/IFOOD; DELIVERY.TP\_STATUS \= ENTREGUE/CANCELADO; ENCOMENDA.TP\_STATUS \= AGENDADA/EM\_PRODUCAO/PRONTA/RETIRADA/CANCELADA/NAO\_RETIRADA; COMANDA.TP\_STATUS \= ABERTA/FECHADA; CAIXA.TP\_STATUS \= FECHADO. Ajuste para os valores reais do seu script. Os códigos G01–G31 são propostos: se o script da seção 3 já tem outra numeração, basta renomear.

## **1.6.1 Informações estratégicas para a gestão**

| Área | O que a gestão consegue saber | Tabelas |
| :---- | :---- | :---- |
| Financeira | Faturamento e fluxo de caixa por período; despesas, contas a pagar e vencidas; diferenças de caixa | VENDA, PAGAMENTO\_VENDA, CONTA\_PAGAR, COMPRA, CAIXA, MOVIMENTACAO\_CAIXA |
| Comercial | Produtos mais vendidos e de maior faturamento; margem por produto; desempenho por canal e ticket médio | VENDA, ITEM\_VENDA, PRODUTO, CATEGORIA |
| Estoque e compras | Itens abaixo do mínimo e rupturas; custo de aquisição; dependência de fornecedores | PRODUTO, MATERIA\_PRIMA, COMPRA, ITEM\_COMPRA, FORNECEDOR |
| Produção | Volume produzido, perdas e desperdícios, produtividade, custo de insumos | PRODUCAO, ITEM\_FICHA\_TECNICA, MATERIA\_PRIMA, FUNCIONARIO |
| Clientes | Clientes frequentes e de maior consumo, inativos, oportunidades de fidelização | CLIENTE, VENDA, COMANDA |
| Encomendas e delivery | Tempo de entrega, entregas no prazo, cancelamentos, desempenho por canal | DELIVERY, VENDA, ENCOMENDA, ITEM\_ENCOMENDA |
| Pessoas e controle interno | Desempenho por funcionário; cancelamentos; divergências de caixa; trilha de auditoria | FUNCIONARIO, VENDA, CANCELAMENTO\_DEVOLUCAO, CAIXA, LOG\_AUDITORIA |

Os quatro problemas da caracterização têm respostas diretas nos dados: **P1** (fluxo de clientes) pelas aberturas de COMANDA por hora; **P2** (consumo sem pagamento) pela diferença entre ITEM\_COMANDA e VENDA; **P3** (gestão informal) pela cobertura de registros no sistema; **P4** (estoque manual) pelos alertas e KPIs de estoque.

## **1.6.2 Indicadores e métricas gerenciais (KPIs)**

Referência para metas: faturamento atual de \~R\$ 190 mil/mês, projeção de \~R\$ 250 mil/mês (RNF11), \~200 clientes e \~50 entregas por dia. *Metas sugeridas, a validar com a gestão.*

| KPI | Fórmula | Período | Meta sugerida |
| :---- | :---- | :---- | :---- |
| Faturamento total | Σ VL\_TOTAL das vendas concluídas | Dia/mês | R\$ 190 mil → R\$ 250 mil/mês |
| Ticket médio | Faturamento ÷ nº de vendas | Dia/mês | ≥ média dos 3 meses anteriores |
| Volume de vendas | Contagem de vendas concluídas | Dia/mês | Crescimento mensal positivo |
| Fluxo de caixa | Entradas (vendas) − saídas (contas pagas) | Dia/mês | Saldo acumulado ≥ 0 |
| Despesas totais | Σ contas pagas (e compras, por competência) | Mês | Dentro do orçamento definido |
| Contas vencidas | Σ valor com vencimento \< hoje e sem pagamento | Semana | R\$ 0 |
| Produtos mais vendidos | Ranking por quantidade e por faturamento | Mês | Informativo |
| Margem bruta por produto | (Preço praticado − custo) × qtd − desconto | Mês | ≥ margem mínima por categoria |
| Taxa de cancelamento de vendas | Vendas canceladas ÷ total de vendas | Mês | ≤ 2% |
| Desempenho por canal | Faturamento, vendas e ticket por TP\_CANAL | Mês | Informativo |
| Estoque abaixo do mínimo | Itens com QT\_ESTOQUE ≤ QT\_ESTOQUE\_MINIMO | Diário | 0 essenciais; ≤ 5% dos demais |
| Índice de ruptura | Produtos ativos com estoque ≤ 0 ÷ produtos ativos | Diário | ≤ 2% (0% nos essenciais) |
| Giro de estoque | Qtd vendida em 30 dias ÷ estoque atual | Mês | Definir por categoria |
| Custo médio de aquisição | Σ(qtd × valor unitário) ÷ Σ qtd | Mês | Variação ≤ inflação setorial |
| Dependência de fornecedores | % das compras por fornecedor | Trimestre | Nenhum fornecedor \> 30% |
| Volume de produção | Σ QT\_PRODUZIDA | Dia/mês | Acompanhar vs. demanda |
| Índice de perdas | Σ QT\_PERDA ÷ Σ QT\_PRODUZIDA | Semana/mês | ≤ 5% |
| Produtividade por funcionário | Σ QT\_PRODUZIDA ÷ dias trabalhados | Mês | Informativo |
| Custo de produção | Σ(qtd da ficha técnica × custo médio do insumo) | Mês | Abaixo do preço com margem mínima |
| Taxa de recorrência | Clientes com mais de 1 compra ÷ clientes identificados | Trimestre | ≥ 60% |
| Ticket médio por cliente | Gasto total ÷ clientes identificados | Trimestre | Crescimento positivo |
| Frequência de compra | Compras ÷ clientes identificados | Trimestre | Crescimento positivo |
| Clientes inativos | Clientes sem compra há mais de 30 dias | Mensal | Reduzir mês a mês |
| Tempo médio de entrega | Média de (DT\_ENTREGA − DT\_SAIDA) | Semana | ≤ 40 min |
| Entregas no prazo | Entregas ≤ 40 min ÷ entregas concluídas | Semana | ≥ 90% |
| Cancelamento de delivery | Entregas canceladas ÷ total | Mês | ≤ 3% |
| Volume de encomendas | Contagem por mês de retirada/entrega | Mês | Informativo |
| Cancelamento de encomendas | Canceladas ÷ total | Mês | ≤ 5% |
| Faturamento por funcionário | Σ VL\_TOTAL por funcionário | Mês | Informativo |
| Cancelamentos por funcionário | Cancelamentos ÷ vendas do funcionário | Mês | Sem desvio em relação à média da equipe |
| Diferenças de caixa | VL\_TOTAL\_INFORMADO − VL\_TOTAL\_SISTEMA | Por fechamento | R\$ 0 (tolerância a definir) |
| Evasão estimada (P2) | Σ (consumido em comanda − cobrado) | Dia/mês | R\$ 0 |
| Cobertura de comanda (P3) | Vendas de balcão com comanda ÷ vendas de balcão | Semana | ≥ 95% |

## **1.6.3 Potencial dos dados para BI**

**Arquitetura sugerida.** O banco operacional (com a implantação híbrida do RNF19) alimenta, por carga noturna ou views, um modelo dimensional. Isso evita que os painéis degradem a resposta de 2 segundos exigida no balcão (RNF01).

•         **Fatos:** vendas (ITEM\_VENDA \+ VENDA \+ PAGAMENTO\_VENDA), compras (ITEM\_COMPRA), produção (PRODUCAO), caixa (CAIXA \+ MOVIMENTACAO\_CAIXA), entregas (DELIVERY).  
•         **Dimensões:** tempo, produto/categoria, cliente, funcionário, canal, fornecedor.

| Visão | Dashboards e relatórios | Consultas |
| :---- | :---- | :---- |
| Financeira | Faturamento × despesas, fluxo de caixa diário, contas a pagar e vencidas | G01–G04 |
| Comercial | Vendas por produto, canal e funcionário; curva ABC; margem | G05–G08, G21 |
| Estoque e compras | Itens críticos, rupturas, custo por fornecedor, concentração | G09–G13 |
| Produção | Produzido × perdido, produtividade, custo de insumos | G14–G16 |
| Clientes | Recorrentes, inativos, ticket por cliente | G17–G18 |
| Logística | Tempo de entrega, prazo, cancelamentos, encomendas | G19–G20 |
| Controle interno | Diferenças de caixa, cancelamentos, auditoria, evasão, adoção do sistema | G22–G27 |

Ferramenta: Power BI, Metabase ou Looker Studio, todas conectáveis ao banco ou a uma réplica de leitura.

## **1.6.4 Aplicações analíticas e de IA**

A IA só entrega valor com histórico acumulado (ideal: 6 a 12 meses). Até lá, começar por regras e médias móveis, e evoluir para modelos de previsão.

| Aplicação | Dados | Técnica sugerida | Consulta de preparação |
| :---- | :---- | :---- | :---- |
| Previsão de vendas e demanda | VENDA, ITEM\_VENDA, PRODUTO | Série temporal com sazonalidade por dia da semana (média móvel → Prophet/ARIMA) | G28 |
| Reposição e produção inteligentes | Previsão \+ PRODUTO, MATERIA\_PRIMA, ITEM\_FICHA\_TECNICA, COMPRA | Previsão × estoque mínimo e consumo semanal (RN19) | G28, G11 |
| Recomendação de produtos | ITEM\_VENDA | Regras de associação (cesta de mercado) | G29 |
| Segmentação e risco de abandono | CLIENTE, VENDA | RFM \+ classificação (churn) | G30 |
| Detecção de anomalias e fraudes | CAIXA, MOVIMENTACAO\_CAIXA, CANCELAMENTO\_DEVOLUCAO, LOG\_AUDITORIA | Z-score / isolation forest | G31, G22, G24 |
| Rentabilidade e simulação de preços | ITEM\_VENDA, PRODUTO, ITEM\_FICHA\_TECNICA | Margem \+ simulação de cenários | G08, G16 |
| Otimização do delivery | DELIVERY, VENDA | Regressão do tempo de entrega por horário e canal | G19 |

## **1.6.5 Rastreabilidade: problema ou KPI → tabelas → consulta**

| Problema ou KPI | Tabelas envolvidas | Consulta |
| :---- | :---- | :---- |
| P1 — sem controle do fluxo de entrada e saída de clientes | COMANDA | G25 |
| P2 — clientes saem sem pagar | COMANDA, ITEM\_COMANDA, VENDA, FUNCIONARIO | G26 |
| P3 — gestão informal (verbal/papel) | VENDA, COMANDA, PRODUCAO, LOG\_AUDITORIA | G27, G14, G24 |
| P4 — estoque totalmente manual | PRODUTO, MATERIA\_PRIMA, ITEM\_VENDA, COMPRA | G09, G10, G11 |
| Faturamento total, ticket médio, volume de vendas | VENDA | G01 |
| Fluxo de caixa | VENDA, CONTA\_PAGAR | G02 |
| Despesas totais | CONTA\_PAGAR, COMPRA | G03 |
| Contas vencidas | CONTA\_PAGAR, FORNECEDOR | G04 |
| Produtos mais vendidos | ITEM\_VENDA, VENDA, PRODUTO | G05 |
| Taxa de cancelamento de vendas | VENDA | G06 |
| Desempenho por canal | VENDA | G07 |
| Margem bruta por produto | ITEM\_VENDA, VENDA, PRODUTO | G08 |
| Estoque abaixo do mínimo | PRODUTO, MATERIA\_PRIMA | G09 |
| Índice de ruptura | PRODUTO | G10 |
| Giro de estoque | PRODUTO, ITEM\_VENDA, VENDA | G11 |
| Custo médio de aquisição | ITEM\_COMPRA, PRODUTO, MATERIA\_PRIMA | G12 |
| Dependência de fornecedores | COMPRA, FORNECEDOR | G13 |
| Volume de produção, índice de perdas | PRODUCAO, PRODUTO | G14 |
| Produtividade por funcionário | PRODUCAO, FUNCIONARIO | G15 |
| Custo de produção | PRODUTO, ITEM\_FICHA\_TECNICA, ITEM\_COMPRA, PRODUCAO | G16 |
| Recorrência, frequência, ticket por cliente | VENDA, CLIENTE | G17 |
| Clientes inativos | CLIENTE, VENDA | G18 |
| Tempo médio e entregas no prazo; cancelamento de delivery | DELIVERY | G19 |
| Volume e cancelamento de encomendas | ENCOMENDA | G20 |
| Faturamento por funcionário | VENDA, FUNCIONARIO | G21 |
| Cancelamentos por funcionário | CANCELAMENTO\_DEVOLUCAO, VENDA, FUNCIONARIO | G22 |
| Diferenças de caixa | CAIXA, FUNCIONARIO | G23 |
| Auditoria de operações | LOG\_AUDITORIA, FUNCIONARIO | G24 |

## **1.6.6 Script SQL das consultas**

\-- G01 Faturamento, volume de vendas e ticket médio por dia  
 SELECT DT\_VENDA::date AS dia, COUNT(\*) AS qt\_vendas, SUM(VL\_TOTAL) AS faturamento,  
    	ROUND(SUM(VL\_TOTAL)/COUNT(\*),2) AS ticket\_medio  
 FROM VENDA WHERE TP\_STATUS='CONCLUIDA' GROUP BY 1 ORDER BY 1;

 \-- G02 Fluxo de caixa diário (entradas \= vendas; saídas \= contas pagas)  
 WITH e AS (SELECT DT\_VENDA::date d, SUM(VL\_TOTAL) v FROM VENDA WHERE TP\_STATUS='CONCLUIDA' GROUP BY 1),  
  	s AS (SELECT DT\_PAGAMENTO d, SUM(VL\_VALOR) v FROM CONTA\_PAGAR WHERE DT\_PAGAMENTO IS NOT NULL GROUP BY 1),  
  	dias AS (SELECT d FROM e UNION SELECT d FROM s)  
 SELECT dias.d AS dia, COALESCE(e.v,0) AS entradas, COALESCE(s.v,0) AS saidas,  
    	COALESCE(e.v,0)-COALESCE(s.v,0) AS saldo,  
    	SUM(COALESCE(e.v,0)-COALESCE(s.v,0)) OVER (ORDER BY dias.d) AS saldo\_acumulado  
 FROM dias LEFT JOIN e ON e.d=dias.d LEFT JOIN s ON s.d=dias.d ORDER BY 1;

 \-- G03 Despesas por mês (contas pagas e compras por competência; não somar as duas, os boletos de compras já estão em CONTA\_PAGAR)  
 SELECT TO\_CHAR(DT\_PAGAMENTO,'YYYY-MM') AS mes, 'CONTAS\_PAGAS' AS origem, SUM(VL\_VALOR) AS total  
 FROM CONTA\_PAGAR WHERE DT\_PAGAMENTO IS NOT NULL GROUP BY 1  
 UNION ALL  
 SELECT TO\_CHAR(DT\_COMPRA,'YYYY-MM'), 'COMPRAS', SUM(VL\_TOTAL) FROM COMPRA GROUP BY 1  
 ORDER BY 1, 2;

 \-- G04 Contas a pagar em aberto e vencidas  
 SELECT f.NM\_FORNECEDOR, c.DS\_DESCRICAO, c.DT\_VENCIMENTO, c.VL\_VALOR,  
    	CASE WHEN c.DT\_VENCIMENTO \< CURRENT\_DATE THEN 'VENCIDA' ELSE 'A\_VENCER' END AS situacao  
 FROM CONTA\_PAGAR c JOIN FORNECEDOR f ON f.ID\_FORNECEDOR=c.ID\_FORNECEDOR  
 WHERE c.DT\_PAGAMENTO IS NULL ORDER BY c.DT\_VENCIMENTO;

 \-- G05 Produtos mais vendidos (quantidade e faturamento)  
 SELECT p.NM\_PRODUTO, SUM(i.QT\_QUANTIDADE) AS qt\_vendida,  
    	SUM(i.QT\_QUANTIDADE\*i.VL\_UNITARIO \- i.VL\_DESCONTO\_ITEM) AS faturamento  
 FROM ITEM\_VENDA i JOIN VENDA v ON v.ID\_VENDA=i.ID\_VENDA JOIN PRODUTO p ON p.ID\_PRODUTO=i.ID\_PRODUTO  
 WHERE v.TP\_STATUS='CONCLUIDA' GROUP BY p.NM\_PRODUTO ORDER BY qt\_vendida DESC LIMIT 10;

 \-- G06 Taxa de cancelamento de vendas por mês  
 SELECT TO\_CHAR(DT\_VENDA,'YYYY-MM') AS mes, COUNT(\*) AS total,  
    	COUNT(\*) FILTER (WHERE TP\_STATUS='CANCELADA') AS canceladas,  
    	ROUND(100.0\*COUNT(\*) FILTER (WHERE TP\_STATUS='CANCELADA')/COUNT(\*),2) AS pct\_cancelamento  
 FROM VENDA GROUP BY 1 ORDER BY 1;

 \-- G07 Desempenho por canal  
 SELECT TP\_CANAL, COUNT(\*) AS qt\_vendas, SUM(VL\_TOTAL) AS faturamento, ROUND(AVG(VL\_TOTAL),2) AS ticket\_medio  
 FROM VENDA WHERE TP\_STATUS='CONCLUIDA' GROUP BY TP\_CANAL ORDER BY faturamento DESC;

 \-- G08 Margem bruta por produto (usa o custo atual de PRODUTO.VL\_CUSTO)  
 SELECT p.NM\_PRODUTO,  
        SUM(i.QT\_QUANTIDADE\*(i.VL\_UNITARIO-p.VL\_CUSTO) \- i.VL\_DESCONTO\_ITEM) AS margem\_bruta,  
        ROUND(100\*SUM(i.QT\_QUANTIDADE\*(i.VL\_UNITARIO-p.VL\_CUSTO) \- i.VL\_DESCONTO\_ITEM)  
          	/ NULLIF(SUM(i.QT\_QUANTIDADE\*i.VL\_UNITARIO),0),1) AS pct\_margem  
 FROM ITEM\_VENDA i JOIN VENDA v ON v.ID\_VENDA=i.ID\_VENDA JOIN PRODUTO p ON p.ID\_PRODUTO=i.ID\_PRODUTO  
 WHERE v.TP\_STATUS='CONCLUIDA' GROUP BY p.NM\_PRODUTO ORDER BY margem\_bruta DESC;

 \-- G09 Produtos e matérias-primas abaixo do estoque mínimo (essenciais primeiro)  
 SELECT 'PRODUTO' AS tipo, NM\_PRODUTO AS nome, QT\_ESTOQUE, QT\_ESTOQUE\_MINIMO, IN\_PRODUTO\_ESSENCIAL AS essencial  
 FROM PRODUTO WHERE IN\_ATIVO AND QT\_ESTOQUE \<= QT\_ESTOQUE\_MINIMO  
 UNION ALL  
 SELECT 'MATERIA\_PRIMA', NM\_MATERIA\_PRIMA, QT\_ESTOQUE, QT\_ESTOQUE\_MINIMO, FALSE  
 FROM MATERIA\_PRIMA WHERE QT\_ESTOQUE \<= QT\_ESTOQUE\_MINIMO  
 ORDER BY 5 DESC, 3;

 \-- G10 Índice de ruptura  
 SELECT COUNT(\*) FILTER (WHERE QT\_ESTOQUE\<=0) AS produtos\_sem\_estoque, COUNT(\*) AS produtos\_ativos,  
    	ROUND(100.0\*COUNT(\*) FILTER (WHERE QT\_ESTOQUE\<=0)/COUNT(\*),2) AS pct\_ruptura  
 FROM PRODUTO WHERE IN\_ATIVO;

 \-- G11 Giro e cobertura de estoque (30 dias; usa o estoque atual como proxy do estoque médio)  
 SELECT p.NM\_PRODUTO, COALESCE(x.qt,0) AS vendido\_30d, p.QT\_ESTOQUE,  
        ROUND(COALESCE(x.qt,0)/NULLIF(p.QT\_ESTOQUE,0),2) AS giro,  
        ROUND(p.QT\_ESTOQUE/NULLIF(COALESCE(x.qt,0)/30.0,0),1) AS dias\_cobertura  
 FROM PRODUTO p  
 LEFT JOIN (SELECT i.ID\_PRODUTO, SUM(i.QT\_QUANTIDADE) AS qt  
        	FROM ITEM\_VENDA i JOIN VENDA v ON v.ID\_VENDA=i.ID\_VENDA  
        	WHERE v.TP\_STATUS='CONCLUIDA' AND v.DT\_VENDA \>= CURRENT\_DATE-30 GROUP BY 1\) x  
    	ON x.ID\_PRODUTO=p.ID\_PRODUTO  
 WHERE p.IN\_ATIVO ORDER BY giro DESC NULLS LAST;

 \-- G12 Custo médio de aquisição (produtos de revenda e matérias-primas)  
 SELECT COALESCE(p.NM\_PRODUTO, m.NM\_MATERIA\_PRIMA) AS item,  
        ROUND(SUM(i.QT\_QUANTIDADE\*i.VL\_UNITARIO)/SUM(i.QT\_QUANTIDADE),2) AS custo\_medio,  
    	MIN(i.VL\_UNITARIO) AS menor, MAX(i.VL\_UNITARIO) AS maior  
 FROM ITEM\_COMPRA i  
 LEFT JOIN PRODUTO p ON p.ID\_PRODUTO=i.ID\_PRODUTO  
 LEFT JOIN MATERIA\_PRIMA m ON m.ID\_MATERIA\_PRIMA=i.ID\_MATERIA\_PRIMA  
 GROUP BY 1 ORDER BY 1;

 \-- G13 Dependência de fornecedores (últimos 90 dias)  
 SELECT f.NM\_FORNECEDOR, SUM(c.VL\_TOTAL) AS total\_comprado,  
        ROUND(100\*SUM(c.VL\_TOTAL)/SUM(SUM(c.VL\_TOTAL)) OVER (),1) AS pct\_do\_total  
 FROM COMPRA c JOIN FORNECEDOR f ON f.ID\_FORNECEDOR=c.ID\_FORNECEDOR  
 WHERE c.DT\_COMPRA \>= CURRENT\_DATE-90 GROUP BY f.NM\_FORNECEDOR ORDER BY total\_comprado DESC;

 \-- G14 Volume de produção e índice de perdas por mês e produto  
 SELECT TO\_CHAR(pr.DT\_PRODUCAO,'YYYY-MM') AS mes, p.NM\_PRODUTO,  
    	SUM(pr.QT\_PRODUZIDA) AS produzido, SUM(pr.QT\_PERDA) AS perdas,  
        ROUND(100\*SUM(pr.QT\_PERDA)/NULLIF(SUM(pr.QT\_PRODUZIDA),0),2) AS pct\_perda  
 FROM PRODUCAO pr JOIN PRODUTO p ON p.ID\_PRODUTO=pr.ID\_PRODUTO  
 GROUP BY 1,2 ORDER BY 1, pct\_perda DESC;

 \-- G15 Produtividade por funcionário (produção)  
 SELECT f.NM\_FUNCIONARIO, f.TP\_CARGO, COUNT(DISTINCT pr.DT\_PRODUCAO::date) AS dias,  
    	SUM(pr.QT\_PRODUZIDA) AS produzido,  
        ROUND(SUM(pr.QT\_PRODUZIDA)/COUNT(DISTINCT pr.DT\_PRODUCAO::date),1) AS media\_por\_dia  
 FROM PRODUCAO pr JOIN FUNCIONARIO f ON f.ID\_FUNCIONARIO=pr.ID\_FUNCIONARIO  
 GROUP BY f.ID\_FUNCIONARIO, f.NM\_FUNCIONARIO, f.TP\_CARGO ORDER BY produzido DESC;

 \-- G16 Custo de produção: insumos por unidade (ficha técnica × custo médio de compra) e total em 30 dias  
 WITH cm AS (SELECT ID\_MATERIA\_PRIMA, SUM(QT\_QUANTIDADE\*VL\_UNITARIO)/SUM(QT\_QUANTIDADE) AS custo  
 	        FROM ITEM\_COMPRA WHERE ID\_MATERIA\_PRIMA IS NOT NULL GROUP BY 1\)  
 SELECT p.NM\_PRODUTO, ROUND(SUM(ft.QT\_QUANTIDADE\*cm.custo),2) AS custo\_insumos\_unit,  
    	ROUND(p.VL\_PRECO\_VENDA \- SUM(ft.QT\_QUANTIDADE\*cm.custo),2) AS margem\_unit,  
    	ROUND(COALESCE(q.qt,0)\*SUM(ft.QT\_QUANTIDADE\*cm.custo),2) AS custo\_producao\_30d  
 FROM PRODUTO p  
 JOIN ITEM\_FICHA\_TECNICA ft ON ft.ID\_PRODUTO=p.ID\_PRODUTO  
 JOIN cm ON cm.ID\_MATERIA\_PRIMA=ft.ID\_MATERIA\_PRIMA  
 LEFT JOIN (SELECT ID\_PRODUTO, SUM(QT\_PRODUZIDA) AS qt FROM PRODUCAO  
        	WHERE DT\_PRODUCAO \>= CURRENT\_DATE-30 GROUP BY 1\) q ON q.ID\_PRODUTO=p.ID\_PRODUTO  
 GROUP BY p.ID\_PRODUTO, p.NM\_PRODUTO, p.VL\_PRECO\_VENDA, q.qt;

 \-- G17 Clientes: recorrência, frequência e ticket por cliente (90 dias, só clientes identificados)  
 WITH c AS (SELECT ID\_CLIENTE, COUNT(\*) AS compras, SUM(VL\_TOTAL) AS gasto  
        	FROM VENDA WHERE TP\_STATUS='CONCLUIDA' AND ID\_CLIENTE IS NOT NULL  
          	AND DT\_VENDA \>= CURRENT\_DATE-90 GROUP BY 1\)  
 SELECT COUNT(\*) AS clientes,  
    	ROUND(100.0\*COUNT(\*) FILTER (WHERE compras\>1)/COUNT(\*),1) AS taxa\_recorrencia,  
    	ROUND(AVG(compras),2) AS frequencia\_media, ROUND(AVG(gasto),2) AS ticket\_por\_cliente  
 FROM c;  
 \-- Top 10 clientes por volume de compras  
 SELECT cl.NM\_CLIENTE, COUNT(\*) AS compras, SUM(v.VL\_TOTAL) AS gasto  
 FROM VENDA v JOIN CLIENTE cl ON cl.ID\_CLIENTE=v.ID\_CLIENTE  
 WHERE v.TP\_STATUS='CONCLUIDA' AND v.DT\_VENDA \>= CURRENT\_DATE-90  
 GROUP BY cl.ID\_CLIENTE, cl.NM\_CLIENTE ORDER BY gasto DESC LIMIT 10;

 \-- G18 Clientes inativos (sem compra há mais de 30 dias)  
 SELECT c.NM\_CLIENTE, c.CD\_TELEFONE, MAX(v.DT\_VENDA)::date AS ultima\_compra,  
    	CURRENT\_DATE \- MAX(v.DT\_VENDA)::date AS dias\_sem\_comprar  
 FROM CLIENTE c JOIN VENDA v ON v.ID\_CLIENTE=c.ID\_CLIENTE  
 WHERE v.TP\_STATUS='CONCLUIDA'  
 GROUP BY c.ID\_CLIENTE, c.NM\_CLIENTE, c.CD\_TELEFONE  
 HAVING MAX(v.DT\_VENDA) \< CURRENT\_DATE-30 ORDER BY dias\_sem\_comprar DESC;

 \-- G19 Delivery por canal: tempo médio, % no prazo (limite de 40 min) e % cancelado  
 SELECT TP\_CANAL\_ORIGEM, COUNT(\*) AS pedidos,  
    	ROUND(AVG(EXTRACT(EPOCH FROM (DT\_ENTREGA-DT\_SAIDA))/60) FILTER (WHERE TP\_STATUS='ENTREGUE'),1) AS tempo\_medio\_min,  
    	ROUND(100.0\*COUNT(\*) FILTER (WHERE TP\_STATUS='ENTREGUE' AND DT\_ENTREGA-DT\_SAIDA \<= INTERVAL '40 minutes')  
          	/ NULLIF(COUNT(\*) FILTER (WHERE TP\_STATUS='ENTREGUE'),0),1) AS pct\_no\_prazo,  
    	ROUND(100.0\*COUNT(\*) FILTER (WHERE TP\_STATUS='CANCELADO')/COUNT(\*),1) AS pct\_cancelado  
 FROM DELIVERY GROUP BY TP\_CANAL\_ORIGEM;

 \-- G20 Encomendas por mês: volume, cancelamento e sinal retido  
 SELECT TO\_CHAR(DT\_RETIRADA\_ENTREGA,'YYYY-MM') AS mes, COUNT(\*) AS volume,  
    	COUNT(\*) FILTER (WHERE TP\_STATUS='CANCELADA') AS canceladas,  
    	ROUND(100.0\*COUNT(\*) FILTER (WHERE TP\_STATUS='CANCELADA')/COUNT(\*),1) AS pct\_cancelamento,  
    	COUNT(\*) FILTER (WHERE TP\_STATUS='NAO\_RETIRADA') AS nao\_retiradas,  
    	SUM(VL\_SINAL) FILTER (WHERE TP\_STATUS='NAO\_RETIRADA') AS sinal\_retido  
 FROM ENCOMENDA GROUP BY 1 ORDER BY 1;

 \-- G21 Faturamento por funcionário  
 SELECT f.NM\_FUNCIONARIO, f.TP\_TURNO, COUNT(\*) AS vendas, SUM(v.VL\_TOTAL) AS faturamento,  
    	ROUND(AVG(v.VL\_TOTAL),2) AS ticket\_medio  
 FROM VENDA v JOIN FUNCIONARIO f ON f.ID\_FUNCIONARIO=v.ID\_FUNCIONARIO  
 WHERE v.TP\_STATUS='CONCLUIDA' GROUP BY f.ID\_FUNCIONARIO, f.NM\_FUNCIONARIO, f.TP\_TURNO  
 ORDER BY faturamento DESC;

 \-- G22 Cancelamentos/devoluções por funcionário (relativo às vendas do próprio funcionário)  
 SELECT f.NM\_FUNCIONARIO, COUNT(cd.ID\_CANCELAMENTO) AS cancelamentos, v.vendas,  
        ROUND(100.0\*COUNT(cd.ID\_CANCELAMENTO)/NULLIF(v.vendas,0),2) AS pct\_cancelamentos  
 FROM FUNCIONARIO f  
 LEFT JOIN CANCELAMENTO\_DEVOLUCAO cd ON cd.ID\_FUNCIONARIO=f.ID\_FUNCIONARIO  
 LEFT JOIN (SELECT ID\_FUNCIONARIO, COUNT(\*) AS vendas FROM VENDA GROUP BY 1\) v ON v.ID\_FUNCIONARIO=f.ID\_FUNCIONARIO  
 GROUP BY f.ID\_FUNCIONARIO, f.NM\_FUNCIONARIO, v.vendas ORDER BY cancelamentos DESC;

 \-- G23 Diferenças de caixa por funcionário responsável pelo fechamento  
 SELECT f.NM\_FUNCIONARIO, COUNT(\*) AS fechamentos,  
    	COUNT(\*) FILTER (WHERE c.VL\_DIFERENCA\<\>0) AS com\_diferenca,  
    	SUM(c.VL\_DIFERENCA) AS saldo\_diferencas, SUM(ABS(c.VL\_DIFERENCA)) AS total\_absoluto  
 FROM CAIXA c JOIN FUNCIONARIO f ON f.ID\_FUNCIONARIO=c.ID\_FUNCIONARIO\_FECHAMENTO  
 WHERE c.TP\_STATUS='FECHADO' GROUP BY f.ID\_FUNCIONARIO, f.NM\_FUNCIONARIO ORDER BY total\_absoluto DESC;

 \-- G24 Auditoria: operações críticas por funcionário, tabela e ação (30 dias)  
 SELECT f.NM\_FUNCIONARIO, l.NM\_TABELA, l.TP\_ACAO, COUNT(\*) AS ocorrencias, MAX(l.DT\_HORA) AS ultima\_ocorrencia  
 FROM LOG\_AUDITORIA l JOIN FUNCIONARIO f ON f.ID\_FUNCIONARIO=l.ID\_FUNCIONARIO  
 WHERE l.DT\_HORA \>= CURRENT\_DATE-30 GROUP BY 1,2,3 ORDER BY ocorrencias DESC;

 \-- G25 (P1) Fluxo de atendimentos por hora do dia e permanência média  
 SELECT EXTRACT(HOUR FROM DT\_ABERTURA) AS hora, COUNT(\*) AS comandas\_abertas,  
    	ROUND(AVG(EXTRACT(EPOCH FROM (DT\_FECHAMENTO-DT\_ABERTURA))/60)  
          	FILTER (WHERE DT\_FECHAMENTO IS NOT NULL),1) AS permanencia\_media\_min  
 FROM COMANDA GROUP BY 1 ORDER BY 1;

 \-- G26 (P2) Comandas em que o cobrado é menor que o consumido (evasão estimada)  
 SELECT cm.ID\_COMANDA, cm.DT\_ABERTURA, cm.TP\_STATUS, f.NM\_FUNCIONARIO,  
        SUM(ic.QT\_QUANTIDADE\*ic.VL\_UNITARIO) AS valor\_consumido,  
    	COALESCE(MAX(v.VL\_TOTAL),0) AS valor\_cobrado,  
        SUM(ic.QT\_QUANTIDADE\*ic.VL\_UNITARIO) \- COALESCE(MAX(v.VL\_TOTAL),0) \- COALESCE(MAX(v.VL\_DESCONTO),0) AS diferenca  
 FROM COMANDA cm  
 JOIN ITEM\_COMANDA ic ON ic.ID\_COMANDA=cm.ID\_COMANDA  
 JOIN FUNCIONARIO f ON f.ID\_FUNCIONARIO=cm.ID\_FUNCIONARIO  
 LEFT JOIN VENDA v ON v.ID\_COMANDA=cm.ID\_COMANDA AND v.TP\_STATUS='CONCLUIDA'  
 WHERE cm.DT\_ABERTURA \< NOW() \- INTERVAL '3 hours'   \-- ignora atendimentos ainda em curso  
 GROUP BY cm.ID\_COMANDA, cm.DT\_ABERTURA, cm.TP\_STATUS, f.NM\_FUNCIONARIO  
 HAVING SUM(ic.QT\_QUANTIDADE\*ic.VL\_UNITARIO) \- COALESCE(MAX(v.VL\_TOTAL),0) \- COALESCE(MAX(v.VL\_DESCONTO),0) \> 0.01  
 ORDER BY diferenca DESC;

 \-- G27 (P3) Adoção do sistema: vendas de balcão com comanda e com cupom fiscal, por dia  
 SELECT DT\_VENDA::date AS dia,  
    	COUNT(\*) FILTER (WHERE TP\_CANAL='BALCAO') AS vendas\_balcao,  
    	ROUND(100.0\*COUNT(\*) FILTER (WHERE TP\_CANAL='BALCAO' AND ID\_COMANDA IS NOT NULL)  
          	/ NULLIF(COUNT(\*) FILTER (WHERE TP\_CANAL='BALCAO'),0),1) AS pct\_com\_comanda,  
    	ROUND(100.0\*COUNT(\*) FILTER (WHERE CD\_CUPOM\_FISCAL IS NOT NULL)/COUNT(\*),1) AS pct\_com\_cupom  
 FROM VENDA WHERE TP\_STATUS='CONCLUIDA' GROUP BY 1 ORDER BY 1;

 \-- G28 (IA) Série diária de quantidade vendida por produto, base para previsão de demanda  
 SELECT v.DT\_VENDA::date AS dia, EXTRACT(DOW FROM v.DT\_VENDA) AS dia\_semana, i.ID\_PRODUTO,  
    	SUM(i.QT\_QUANTIDADE) AS qt\_vendida  
 FROM ITEM\_VENDA i JOIN VENDA v ON v.ID\_VENDA=i.ID\_VENDA  
 WHERE v.TP\_STATUS='CONCLUIDA' GROUP BY 1,2,3 ORDER BY 3,1;

 \-- G29 (IA) Pares de produtos comprados juntos (cesta de mercado)  
 SELECT a.ID\_PRODUTO AS produto\_a, b.ID\_PRODUTO AS produto\_b, COUNT(\*) AS vendas\_em\_conjunto  
 FROM ITEM\_VENDA a JOIN ITEM\_VENDA b ON b.ID\_VENDA=a.ID\_VENDA AND a.ID\_PRODUTO \< b.ID\_PRODUTO  
 GROUP BY 1,2 HAVING COUNT(\*) \>= 10 ORDER BY 3 DESC LIMIT 50;

 \-- G30 (IA) RFM de clientes (notas de 1 a 5\)  
 SELECT ID\_CLIENTE, CURRENT\_DATE \- MAX(DT\_VENDA)::date AS recencia\_dias, COUNT(\*) AS frequencia,  
    	SUM(VL\_TOTAL) AS valor,  
    	NTILE(5) OVER (ORDER BY MAX(DT\_VENDA)) AS nota\_r,  
    	NTILE(5) OVER (ORDER BY COUNT(\*)) AS nota\_f,  
    	NTILE(5) OVER (ORDER BY SUM(VL\_TOTAL)) AS nota\_m  
 FROM VENDA WHERE TP\_STATUS='CONCLUIDA' AND ID\_CLIENTE IS NOT NULL GROUP BY ID\_CLIENTE;

 \-- G31 (IA) Fechamentos de caixa atípicos (\|z-score\| \>= 2\)  
 WITH s AS (SELECT AVG(VL\_DIFERENCA) AS mu, STDDEV(VL\_DIFERENCA) AS sd FROM CAIXA WHERE TP\_STATUS='FECHADO')  
 SELECT c.ID\_CAIXA, c.DT\_FECHAMENTO, c.ID\_FUNCIONARIO\_FECHAMENTO, c.VL\_DIFERENCA,  
    	ROUND((c.VL\_DIFERENCA \- s.mu)/NULLIF(s.sd,0),2) AS z\_score  
 FROM CAIXA c CROSS JOIN s  
 WHERE c.TP\_STATUS='FECHADO' AND s.sd \> 0 AND ABS(c.VL\_DIFERENCA \- s.mu) \>= 2\*s.sd  
 ORDER BY ABS(c.VL\_DIFERENCA \- s.mu) DESC;

## **1.6.7 Limitações do modelo que afetam os KPIs**

| Limitação | Efeito | Ajuste sugerido |
| :---- | :---- | :---- |
| ENCOMENDA não tem VL\_TOTAL nem data de criação (o conceitual prevê VL\_TOTAL) e não se liga a VENDA | Não há faturamento nem prazo de antecedência de encomendas; o saldo de 80% só entra no faturamento se for lançado como venda | Incluir VL\_TOTAL e DT\_ENCOMENDA, ou lançar a retirada como VENDA |
| DELIVERY não tem prazo prometido nem distância | "Entrega no prazo" usa limite fixo de 40 min em G19; a regra de raio (RN12) não é auditável | Incluir DT\_PREVISTA e VL\_DISTANCIA\_KM |
| Não há histórico de movimentação de estoque | Giro usa o estoque atual como proxy do estoque médio (G11) | Registrar movimentações ou snapshots diários |
| PRODUTO.VL\_CUSTO guarda só o custo atual | Margem histórica (G08) é aproximada | Gravar o custo em ITEM\_VENDA ou manter histórico de preços |
| CAIXA não tem turno | Diferenças por turno dependem do horário de DT\_FECHAMENTO (14h/22h) | Derivar o turno na consulta ou incluir TP\_TURNO |
| CLIENTE é opcional em VENDA | KPIs de clientes (G17, G18, G30) cobrem só vendas identificadas | Incentivar identificação por telefone nas vendas de maior valor |
| CONTA\_PAGAR não se liga a COMPRA (RN23) | Somar compras e contas pagas duplicaria despesas | G03 mostra as duas visões separadas |
| CANCELAMENTO\_DEVOLUCAO.ID\_FUNCIONARIO é quem registra/autoriza | G22 mede quem autoriza, não necessariamente quem vendeu | Confirmar a regra com a gestão |

---

## 2\. Modelo Lógico Relacional
  A seguir, link de acesso ao modelo lógico Relacional: https://app.brmodeloweb.com/publicview/6aecf0ba676def35996efc0e
---

## 3\. Script SQL Completo
  Acesse [aqui](/Docs/script_padaria_delicia_ii.sql) o script .sql completo.
---


## 4\. Massa de Dados
  Acesse a massa de dados [aqui](/Docs/script_padaria_delicia_ii.sql).
---


## 5\. Uso de Inteligência Artificial

| Item | O que registrar |
| :---- | :---- |
| **Ferramenta e etapa** | Gemini foi usado para melhoria de imagem na parte de comprovação das entrevistas (segunda reunião). |
| **Motivação** | Fundo do print, não profissional, também dificultando a leitura.  |
| **Prompt(s) utilizados** | “Olá Gemini\!Poderia me ajudar a deixar o fundo da imagem neutro e profissional?” |
| **Resposta recebida** | Arquivo exato Agendamento\_Whatsapp.jpeg. |
| **Fontes consultadas e verificadas** | Imagem sem edição. |
| **Trechos rejeitados ou corrigidos** | Nada rejeitado ou corrigido. |
| **Justificativa da escolha final** | O grupo manteve, pois foi entregue o desejado. |
| **Reflexão crítica** | O grupo acredita que o Gemini é um dos melhores softwares para manipular imagens. Pois não foi preciso ajustes. |

| Item | O que registrar |
| :---- | :---- |
| **Ferramenta e etapa** | Claude (antropic) foi utilizado na parte Revisão do Modelo Conceitual. |
| **Motivação** | Para organização dos pontos citados em prompt. |
| **Prompt(s) utilizados** | “Olá Claude\!Pode me ajudar a organizar no estilo da tabela de exemplo as mudanças do DER. De forma resumida, foram corrigidos todos os pontos de melhoria citados no feedback da entrega 1\. |
| **Resposta recebida** | Foi entregue uma tabela com todos os pontos citados no prompt de forma organizada. |
| **Fontes consultadas e verificadas** | Foram fornecidos: tabela de exemplo desejada, o dicionário atualizado, arquivo de feedback da entrega 1 e o DER antigo e atualizado. |
| **Trechos rejeitados ou corrigidos** | Foram retirados pontos desnecessários que não foram mudanças diretamente do DER, mas sim do dicionário. |
| **Justificativa da escolha final** | Foi adaptado por conta de terem pontos não desejados e condizentes com o pedido. |
| **Reflexão crítica** | Não foram identificados vieses explícitos, pois o grupo deixou exemplificado sobre o resultado desejado, mas é fundamental a revisão da resposta. |

| Item | O que registrar |
| :---- | :---- |
| **Ferramenta e etapa** | Claude foi utilizado para revisão do modelo conceitual e lógico. |
| **Motivação** | Agilidade e retorno de melhorias em um prompt. |
| **Prompt(s) utilizados** | “Olá Claude\!De acordo com o histórico do nosso projeto e materiais fornecidos, poderia avaliar e falar pontos faltantes nos modelos conceitual e lógico?” |
| **Resposta recebida** | DER correto e alguns pontos a melhorar no lógico, como falta de atenção no alinhamento com o dicionário na parte de classificação dos atributos. |
| **Fontes consultadas e verificadas** | Documento dos fatores necessários entrega 2, arquivos DER e modelo lógico atualizados. |
| **Trechos rejeitados ou corrigidos** | Foi verificado se os erros faziam sentido e foram corrigidos no escopo do trabalho. |
| **Justificativa da escolha final** | Tudo que foi apontado eram erros coerentes. |
| **Reflexão crítica** | IA sem vieses, apontou erros verdadeiros cometidos pelo grupo. Claude é ótimo para revisões. |

| Item | O que registrar |
| :---- | :---- |
| **Ferramenta e etapa** | Claude foi usado para revisão dos requisitos e regras de negócio. |
| **Motivação** | O grupo recorreu à IA por conta de não saber literalmente todos os processos comuns de uma panificadora. |
| **Prompt(s) utilizados** | “Olá Claude\!Pode nos ajudar falando quais os processos comuns faltantes nos requisitos e regras de negócio? Nos dê sugestões do que adicionar. |
| **Resposta recebida** | Maiores detalhamentos em contas a pagar, LGPD e conferências de mercadoria. |
| **Fontes consultadas e verificadas** | Repositório. |
| **Trechos rejeitados ou corrigidos** | API Ifood, registros de multas e integração com equipamentos. |
| **Justificativa da escolha final** | Não foi adicionado tudo que a IA deu de sugestão pois já estava explicado em outras partes do escopo do trabalho. |
| **Reflexão crítica** | Mesmo falando que não precisava de integração com Ifood, a IA sugeriu. Além de fatos já discutidos em histórico. Acredita-se que isso ocorreu por conta da falta de tokens e configuração de esforço baixo. |

| Item | O que registrar |
| :---- | :---- |
| **Ferramenta e etapa** | Claude (Anthropic), usado na etapa de documentação do projeto de banco de dados, na elaboração da justificativa técnica do modelo conceitual e lógico. |
| **Motivação** | Converter os diagramas em um texto técnico organizado em quatro eixos (chaves primárias, estrangeiras, restrições e arquitetura), ganhando velocidade  |
| **Prompt(s) utilizados** | Envio dos PDFs dos modelos conceitual e lógico (BRModelo Web) com os quatro tópicos exigidos, incluindo o ajuste da entidade `ITEM_ENCOMENDA`, solicitando uma justificativa técnica estruturada por blocos funcionais.  |
| **Resposta recebida** | Documento com introdução, justificativas de PKs (chaves substitutas e alternativas `UNIQUE`), FKs divididas por blocos, listagem de restrições de integridade, decisões arquiteturais e pontos de atenção.  |
| **Fontes consultadas e verificadas** | O conteúdo vem do diagrama enviado e de conhecimento geral de modelagem.  |
| **Trechos rejeitados ou corrigidos** | Foi atualizado em relação a entidade ITEM\_ENCOMENDA. |
| **Justificativa da escolha final** | Preservação da estrutura de quatro eixos da avaliação, adoção de chaves substitutas com `UNIQUE` para estabilidade cadastral, e vínculo das restrições a regras de negócio para rastreabilidade.  |
| **Reflexão crítica** | A IA agilizou a estruturação e apontou detalhes lógicos complexos (como arcos exclusivos e FKs 1:1), mas exigiu rigorosa conferência humana para corrigir ambiguidades visuais dos diagramas e validar as regras de negócio geradas. O uso serviu como rascunho avançado, reforçando que a decisão técnica final é sempre humana.  |

| Item | O que registrar |
| :---- | :---- |
| **Ferramenta e etapa** | Claude foi utilizado na etapa de revisão e documentação do banco de dados, para analisar o modelo conceitual, o modelo lógico e o dicionário de dados |
| **Motivação** | Havia dúvida sobre a clareza dos documentos do projeto, principalmente quanto à conversão de entidades, atributos, relacionamentos e cardinalidades. |
| **Prompt(s) utilizados** | Compare o modelo conceitual com o modelo lógico e liste as inconsistências (número de entidades, tabelas ausentes, chaves primárias e tipos de dados), informando em qual arquivo está cada uma. |
| **Resposta recebida** | O conceitual tem 24 entidades e o lógico tem 23, pois a ITEM\_ENCOMENDA aparece só no conceitual. A IA também apontou diferenças na ENCOMENDA, na chave primária de PAGAMENTO\_VENDA e nos tipos de dados (FLOAT no lógico e decimal no dicionário). |
| **Fontes consultadas e verificadas** | A IA não buscou nada de fora, somente os arquivos que estão dentro do projeto |
| **Trechos rejeitados ou corrigidos** | A IA disse que o modelo lógico tinha 24 tabelas, mas ao contar uma por uma deu 23\. Ela também disse que o lógico tinha “perdido” a ITEM\_ENCOMENDA, mas o dicionário mostrou que o desatualizado era o conceitual. As duas informações foram corrigidas antes de entrar no projeto |
| **Justificativa da escolha final** | O resultado bateu com os arquivos quando conferido e deixou claro o que corrigir antes da entrega. |
| **Reflexão crítica** | O Claude achou diferenças que passariam despercebidas, mas errou em dados básicos. Por isso, a resposta precisa ser conferida. |

| Item | O que registrar |
| :---- | :---- |
| **Ferramenta e etapa** | Claude foi utilizado para comparar elementos do dicionário e do README |
| **Motivação** | Havia uma grande quantidade de entidades, cardinalidades e atributos para verificar oque tinha no dicionário a qual não estava no README |
| **Prompt(s) utilizados** | Compare as entidades, cardinalidades e atributos presentes no dicionário de dados com os descritos no README e aponte o que está faltando no README. |
| **Resposta recebida** | Entidades**:** o dicionário tem 23 e o README tem 20\. Faltam no README PAGAMENTO\_VENDA, DELIVERY e CONTA\_PAGAR. |
| **Fontes consultadas e verificadas** | A IA não usou fontes externas. Todas as informações vieram dos próprios documentos do grupo (o index.html e o texto do README), então a verificação foi comparar a resposta com esses dois arquivos. |
| **Trechos rejeitados ou corrigidos** | Quando pedimos só os atributos que não estavam no README, a primeira lista veio com nomes que já existem no README em outras entidades, como TP\_STATUS, ID\_VENDA e VL\_VALOR. Tivemos que pedir de novo até ficar só com os atributos realmente novos. |
| **Justificativa da escolha final** | Mantivemos o resultado das comparações porque ele bateu com o que vimos no dicionário e no README quando conferimos. A divisão em entidades, cardinalidades e atributos também deixou fácil enxergar o que precisa ser atualizado no README. |
| **Reflexão crítica** | A IA foi útil para achar rapidamente diferenças que a gente talvez não percebesse, mas teve limites. Ela errou ao afirmar uma coisa sobre o relacionamento de PRODUTO antes de ler o dicionário por completo, e demorou a entender o que queríamos nos atributos, o que mostra que ela depende de a gente explicar bem o pedido |

| Item | O que registrar |
| :---- | :---- |
| **Ferramenta e etapa** | Claude foi usado para revisão dos requisitos e regras de negócio. |
| **Motivação** | O grupo recorreu à IA por conta de não saber literalmente todos os processos comuns de uma panificadora. |
| **Prompt(s) utilizados** | “Olá Claude\!Pode nos ajudar falando quais os processos comuns faltantes nos requisitos e regras de negócio? Nos dê sugestões do que adicionar. |
| **Resposta recebida** | Maiores detalhamentos em contas a pagar, LGPD e conferências de mercadoria. |
| **Fontes consultadas e verificadas** | Repositório. |
| **Trechos rejeitados ou corrigidos** | API Ifood, registros de multas e integração com equipamentos. |
| **Justificativa da escolha final** | Não foi adicionado tudo que a IA deu de sugestão pois já estava explicado em outras partes do escopo do trabalho. |
| **Reflexão crítica** | Mesmo falando que não precisava de integração com Ifood, a IA sugeriu. Além de fatos já discutidos em histórico. Acredita-se que isso ocorreu por conta da falta de tokens e configuração de esforço baixo. |

| Item | O que registrar |
| :---- | :---- |
| **Ferramenta e etapa** | Chatgpt usado para script SQL, sendo executado de acordo com as instruções |
| **Motivação** | O grupo recorreu à IA por conta de não ter domínio em SQL. |
| **Prompt(s) utilizados** | “Olá Chat, poderia me ajudar a criar e executar sql de acordo com o material?\! |
| **Resposta recebida** | Código SQL do repositório. |
| **Fontes consultadas e verificadas** | Repositório. |
| **Trechos rejeitados ou corrigidos** | Tudo executado, nada removido. |
| **Justificativa da escolha final** | Estava sendo executado perfeitamente. |
| **Reflexão crítica** | C |

---

## 6\. Apresentação Corporativa Final
  Acesse a apresentação em slides [aqui](Slides_Padaria_Delícia_II.pptx).
---
