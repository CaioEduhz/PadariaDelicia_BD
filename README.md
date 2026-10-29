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

Além disso, há um registro anexado abaixo da reunião de levantamento de dados realizada por videoconferência com o gerente da empresa. Embora um dos integrantes do grupo frequente presencialmente o estabelecimento, a reunião foi realizada nesse formato devido à preferência do responsável pela padaria, considerando a disponibilidade de agendas dos envolvidos.

[Print de reunião com um dos donos da padaria](img/Reuniao.jpg)

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
- **Objetivo:** Quitar os compromissos financeiros gerados pelas aquisições de insumos e mercadorias de revenda.
- **Atores:** Sócio / Administrador, Fornecedores.
- **Entrada / Gatilho:** Vencimento dos boletos ou faturas emitidas pelos fornecedores.
- **Fluxo Principal:**
  1. O Sócio confere os boletos bancários relativos às compras de insumos e mercadorias recebidas.
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

- **RN05 — Pedido sob encomenda (Antecedência e Multa por Não Retirada).** Solicitações de encomendas exigem antecedência mínima de 1 dia. O pagamento é realizado na retirada, porém, caso o cliente não compareça para retirar a encomenda produzida, aplica-se uma cobrança de 20% do valor total a título de multa para cobrir os custos de produção e evitar prejuízos ao estabelecimento.

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

- **RF28 - Venda rápida por código de barras/balança.** Leitura de código de barras e integração com balança para produtos pesados.

- **RF29 - Controle de desperdício e perdas.** Registro de perdas (vencimento, quebra, sobra do dia) com motivo. Nesse caso,alimenta o relatório de desperdício do RF21.

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

### 4.1 Regras operacionais  
*Condições que a organização impõe sobre suas transações do dia a dia.*

- **RN01 — Produto vencido.** Produtos com prazo de validade expirado não podem ser vendidos.

- **RN02 — Estoque insuficiente.** Bloqueia venda acima do disponível em estoque, salvo autorização expressa do administrador/sócio.

- **RN03 — Desconto e alçadas.** Descontos acima de um percentual definido exigem autorização exclusiva dos sócios.

- **RN04 — Cancelamento restrito.** Cancelamentos após o fechamento de caixa exigem autorização do gerente ou administrador.

- **RN05 — Pedido sob encomenda (Sinal e Antecedência).** Exige antecedência mínima de 24 horas, registro de itens, data e horário de retirada, com cobrança prévia de 20% do valor como sinal; caso o cliente não retire o pedido, o sinal é retido para cobrir os custos de insumos.

- **RN06 — Produção diária e perdas.** A produção diária inicia às 06h, sendo obrigatório o registro formal no sistema dos lotes produzidos, sobras do dia anterior e eventuais perdas ou descartes.

- **RN07 — Fechamento e divergência de caixa.** O fechamento de caixa é obrigatório duas vezes ao dia (às 14h e às 22h), comparando o valor esperado pelo sistema com o contado em dinheiro; qualquer diferença exige o registro do motivo e auditoria/ajuste pelo sócio.

- **RN08 — Comanda e controle de atendimento no salão.** Toda comanda de salão ou balcão deve possuir número único, data e hora de abertura, local de consumo, e conter itens detalhados com quantidade ou peso (em kg para produtos vendidos a granel) e valores; o encerramento do atendimento exige o pagamento integral registrado.

- **RN09 — Prioridade de produtos críticos.** Produtos essenciais (ex.: pão francês) geram alertas de estoque mínimo com prioridade de reposição sobre os demais.

- **RN10 — Área e taxa de entrega (Delivery).** O delivery próprio via WhatsApp limita-se a um raio máximo de até 5 km com taxa fixa de R$ 4,00; pedidos via aplicativo (iFood) atendem até 10 km, com taxa calculada pela plataforma, sendo vedada a aceitação de pedidos fora do raio e exigindo pagamento exclusivamente eletrônico.

- **RN11 — Formas de pagamento e fracionamento.** O caixa aceita dinheiro, PIX e cartões, permitindo a divisão de uma mesma conta entre diferentes clientes ou modalidades de pagamento.

- **RN12 — Conferência e rejeição no recebimento de insumos.** Toda mercadoria entregue por fornecedores passa por inspeção física obrigatória antes da estocagem; havendo divergências de quantidade, avarias ou produtos vencidos, a carga é rejeitada com o respectivo motivo registrado.

- **RN13 — Cálculo de compras e reposição de estoque.** A quantidade a comprar é calculada pela média de consumo semanal menos o estoque remanescente (não podendo ser negativa; se o estoque atual for maior ou igual à média semanal, nenhum pedido de compra é gerado).

- **RN14 — Gestão financeira e quitação de passivos.** Os boletos de fornecedores são pagos na data de vencimento acordada e devem estar vinculados a uma compra registrada com fornecedor e valores conferidos; as despesas operacionais gerais são liquidadas dentro do prazo com arquivamento de comprovantes.

- **RN15 — Gestão de turnos, acessos e operações críticas.** O 1º turno opera das 06h às 14h e o 2º turno das 14h às 22h; o acesso ao sistema baseia-se em perfis por cargo, sendo que operações críticas (alterar preços, cadastrar produtos, aplicar descontos, cancelar itens e visualizar relatórios) registram obrigatoriamente o usuário, a data e a hora, sendo exclusivas dos sócios.

### 4.2 Restrições organizacionais, legais e fiscais
Limitações externas ou internas que afetam o modelo — e por que importam.

- **RN16 — Documentação regulatória obrigatória**: Todo produto e o estabelecimento devem ter vinculados os documentos regulatórios aplicáveis, incluindo ficha técnica, laudos de bombeiro e sanitário, alvará de funcionamento e código de defesa do consumidor. A ausência dos documentos exigidos pode gerar multas ou interdição.
- **RN17 — Proteção de dados pessoais (LGPD)**: Os dados de clientes, como nome e telefone, são considerados dados pessoais. O sistema exige controle de acesso, finalidade clara de uso (marketing pontual de produtos recém-produzidos) e validação de CPF único quando fornecido voluntariamente pelo cliente.
- **RN18 — Emissão obrigatória de documento fiscal**: Toda venda concluída deve gerar o respectivo cupom ou nota fiscal por meio do sistema Saurus, garantindo a conformidade tributária.
- **RN19 — Proibição de venda de bebidas alcoólicas a menores**: É proibida a comercialização de bebidas alcoólicas a menores de 18 anos. A maioridade deve ser verificada por documento de identificação sempre que o cliente apresentar aparência jovem, conforme a legislação vigente.
---

**## 5. Dicionário de Dados Conceitual (Preliminar)**  

Confira o [Dicionário de Dados](Dicionario/index.html) feito no software BRMW BrModelo.

---

**## 6. Modelagem Conceitual (Entidades, Atributos, Relacionamentos)**  

-  **Entidades reconhecidas:**

**CATEGORIA**: utilizada para organizar os produtos em grupos, como pães, confeitaria, bebidas e salgados. 

**PRODUTO:** representa os produtos acabados comercializados pela padaria, sejam de produção própria ou de revenda.  

**MATERIA\_PRIMA:** representa os insumos utilizados na produção, como farinha, ovos e fermento, que possuem controle de estoque próprio.  

**ITEM\_FICHA\_TECNICA:** entidade associativa que relaciona produtos e matérias-primas, registrando a quantidade de cada insumo necessária para produzir determinado produto.  

**DOCUMENTO\_REGULATORIO:** armazena documentos e laudos regulatórios, podendo estar relacionado a um produto específico ou ao estabelecimento.  

**FORNECEDOR:** representa as pessoas ou empresas responsáveis pelo fornecimento de produtos e matérias-primas.  

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

**ENCOMENDA:** representa pedidos antecipados ou personalizados. Essa entidade foi mantida no modelo por completude, porém possui baixa prioridade e não foi confirmada pela gestão durante a pesquisa de campo.  


-  **Atributos e classificações:** quais atributos pertencem a cada entidade.  

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

**CATEGORIA:** ID\_CATEGORIA, NM\_CATEGORIA.  

**PRODUTO:** ID\_PRODUTO, CD\_PRODUTO, NM\_PRODUTO, DS\_PRODUTO, ID\_CATEGORIA, VL\_PRECO\_VENDA, VL\_CUSTO, CD\_UNIDADE, QT\_ESTOQUE, QT\_ESTOQUE\_MINIMO, DT\_VALIDADE, IN\_PRODUTO\_ESSENCIAL, IN\_ATIVO.  

**MATÉRIA\_PRIMA:** ID\_MATERIA\_PRIMA, NM\_MATERIA\_PRIMA, CD\_UNIDADE, QT\_ESTOQUE, QT\_ESTOQUE\_MINIMO.  

**ITEM\_FICHA\_TECNICA:** ID\_PRODUTO, ID\_MATERIA\_PRIMA, QT\_QUANTIDADE.  

**DOCUMENTO\_REGULATORIO:** ID\_DOCUMENTO, TP\_DOCUMENTO, DS\_ARQUIVO, DT\_EMISSAO, DT\_VALIDADE, ID\_PRODUTO.  

**FORNECEDOR:** ID\_FORNECEDOR, NM\_FORNECEDOR, CD\_CNPJ\_CPF, NM\_CONTATO, CD\_TELEFONE, DS\_ENDERECO.  

**COMPRA:** ID\_COMPRA, ID\_FORNECEDOR, DT\_COMPRA, CD\_NOTA\_FISCAL, VL\_TOTAL.  

**ITEM\_COMPRA:** ID\_COMPRA, ID\_ITEM, ID\_PRODUTO, ID\_MATERIA\_PRIMA, QT\_QUANTIDADE, VL\_UNITARIO.  

**PRODUCAO**  ID\_PRODUCAO, ID\_PRODUTO, ID\_FUNCIONARIO, DT\_PRODUCAO, QT\_PRODUZIDA.  

**CLIENTE:** ID\_CLIENTE, NM\_CLIENTE, CD\_TELEFONE, DT\_CADASTRO.  

**FUNCIONARIO:** ID\_FUNCIONARIO, NM\_FUNCIONARIO, TP\_CARGO, TP\_TURNO, CD\_LOGIN, DS\_SENHA\_HASH, IN\_ATIVO.  

**COMANDA:** ID\_COMANDA, ID\_CLIENTE, ID\_FUNCIONARIO, DT\_ABERTURA, DT\_FECHAMENTO, TP\_STATUS.  

**ITEM\_COMANDA:** ID\_COMANDA, ID\_ITEM, ID\_PRODUTO, QT\_QUANTIDADE, VL\_UNITARIO.  

**VENDA:** ID\_VENDA, DT\_VENDA, ID\_CAIXA, ID\_FUNCIONARIO, ID\_CLIENTE, ID\_COMANDA, TP\_CANAL, TP\_FORMA\_PAGAMENTO, VL\_TOTAL, VL\_DESCONTO, VL\_RECEBIDO, VL\_TROCO, TP\_STATUS.  

**ITEM\_VENDA:** ID\_VENDA, ID\_ITEM, ID\_PRODUTO, QT\_QUANTIDADE, VL\_UNITARIO, VL\_DESCONTO\_ITEM.  

**CANCELAMENTO\_DEVOLUCAO:** ID\_CANCELAMENTO, ID\_VENDA, ID\_PRODUTO, DT\_CANCELAMENTO, DS\_MOTIVO, ID\_FUNCIONARIO.  

**CAIXA:** ID\_CAIXA, ID\_FUNCIONARIO\_ABERTURA, DT\_ABERTURA, ID\_FUNCIONARIO\_FECHAMENTO, DT\_FECHAMENTO, VL\_ABERTURA, VL\_TOTAL\_SISTEMA, VL\_TOTAL\_INFORMADO, VL\_DIFERENCA, TP\_STATUS.  

**MOVIMENTACAO\_CAIXA:** ID\_MOVIMENTACAO, ID\_CAIXA, TP\_MOVIMENTACAO, VL\_VALOR, DT\_HORA, DS\_MOTIVO, ID\_FUNCIONARIO.  

**LOG\_AUDITORIA:** ID\_LOG, ID\_FUNCIONARIO, NM\_TABELA, TP\_ACAO, DT\_HORA, DS\_DETALHE.  

**ENCOMENDA:** ID\_ENCOMENDA, ID\_CLIENTE, ID\_PRODUTO, DT\_RETIRADA\_ENTREGA, TP\_STATUS, DS\_PERSONALIZACAO.   

 
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

**PRODUTO — ENCOMENDA:** um produto pode estar associado a várias encomendas (1:N).  
 

- **Restrições e políticas organizacionais aplicadas ao modelo.**

**Produto vencido:** produtos vencidos não podem ser vendidos.  
**Estoque insuficiente:** a venda acima do estoque disponível deve ser bloqueada, salvo autorização do administrador.  
**Desconto:** descontos acima do percentual definido exigem autorização.  
**Cancelamento:** cancelamentos realizados após o fechamento do caixa exigem autorização do gerente ou administrador.  
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

O Diagrama Entidade-Relacionamento (DER) foi anexado junto ao repositório, confira [***aqui***](DER_Padaria_Delicia_II.png).

---

**## 8. Justificativa Técnica**  

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

ENCOMENDA isolada e marcada como não confirmada.Mantivemos a entidade por completude e por ser típica do segmento, mas sem ligá-la a VENDA ou COMANDA,e com a personalização concentrada em um único campo de texto livre (DS\_PERSONALIZACAO) em vez de atributos estruturados para sabor, tamanho,recheio e tema.A razão é de honestidade metodológica: estruturar detalhadamente um requisito que a gestão não validou em campo produziria um modelo aparentemente mais completo, porém apoiado em suposições nossas e não em evidência.

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

Algumas estruturas usuais em sistemas de varejo foram conscientemente não modeladas: catálogo fornecedor × produto, entidade de ajuste manual de estoque e desperdício, e indicadores calculados do dashboard (RF22), que são derivações de consulta e não dados a persistir.Em todos os casos o raciocínio foi o mesmo o ganho informacional não compensa o custo de alimentação manual em uma operação de 12 pessoas, e o histórico já existente (ITEM\_COMPRA, por exemplo) responde à necessidade prática. O modelo tem 19 entidades porque esse é o número necessário para cobrir os requisitos confirmados; uma versão com 30 entidades seria mais impressionante no papel e menos utilizável no balcão.

***8.6 Limitações assumidas***

Registramos duas limitações conhecidas do recorte atual. A validade está modelada como atributo de PRODUTO (DT\_VALIDADE), o que pressupõe uma validade vigente por produto e não permite rastrear lotes distintos do mesmo item suficiente para o bloqueio exigido pela RN01 no contexto de uma padaria com giro diário, mas insuficiente caso a empresa passe a trabalhar com estoques de validade longa. E a baixa de matéria-prima pela produção usa a quantidade prevista na ficha técnica, não a quantidade efetivamente consumida, o que não captura perdas de produção decisão coerente com a despriorização do relatório de desperdício pela gestão, e revisitável na mesma estrutura já usada em MOVIMENTACAO\_CAIXA.

---

**## 9. Uso de Inteligência Artificial**  

Se o grupo usou alguma ferramenta de IA (ChatGPT, Claude, Gemini, Perplexity etc.) em qualquer parte do trabalho, registre **para cada uso relevante**:

| Item | O que registrar |
|------|------------------|  
| **Ferramenta e etapa** | Claude foi usado para a escritura da introdução. |  
| **Motivação** | Para mapeamento de informações relevantes e para a revisão de dados faltantes. |  
| **Prompt(s) utilizados** |  Olá, claude\! Faça uma introdução com base nesse texto, no arquivo da entrevista e nas informações dos links que eu te mandar. Me fale as informações faltantes para uma introdução completa: * Nome e natureza da organização: *qual organização real o grupo escolheu (com acesso garantido para pesquisa de campo) — pode ser uma empresa (livraria, lanchonete, pet shop), uma ONG, uma associação comunitária ou outra instituição.*   * Contexto e porte: *com ou sem fins lucrativos; tamanho da operação; número de pessoas envolvidas (funcionários, voluntários, membros, fiéis); volume de atividades (vendas, atendimentos, doações, rituais, eventos).*   * Problemas e necessidades identificados: *qual é a "crise operacional" — o que está desorganizado hoje (planilhas soltas, papel, falta de controle de estoque/doações/cadastros, etc.)?*   * Justificativa da escolha: *por que essa organização foi escolhida e por que ela é um bom caso para o projeto?*   * Evidências da organização: *comprove que a organização existe e que o grupo teve acesso a ela — ex.: fotos do local/da visita, link da organização no Google (Google Maps/Google Meu Negócio, site, rede social), endereço completo e forma de contato (telefone, e-mail, responsável pela organização).* https://wanderboat.ai/restaurants/brazil/regi%C3%A3o-imediata-de-s%C3%A3o-paulo/padaria-del%C3%ADcia/pFyXZmTJSiejlYw5WReEkg; https://www.solutudo.com.br/empresas/sp/s-paulo/panificadoras-e-padarias/padaria-delicia-ii-ltda-17960017; https://cnpj.biz/13565217000190; https://cnpja.com/office/13565217000190; https://www.instagram.com/delicia2padaria?igsi=cDIxa3dnMmhtMHly; documento do levantamento de dados com o dono da empresa |
| **Resposta recebida** | A Padaria Delícia II Ltda., nome fantasia Padaria Delícia II, é uma microempresa com fins lucrativos localizada na Rua Impata, nº 765, Vila Ré, São Paulo-SP, CEP 03663-010. Inscrita no CNPJ 13.565.217/0001-90, iniciou suas atividades em 19 de abril de 2011 e encontra-se com situação cadastral ativa. Sua atividade principal é o comércio varejista de produtos de padaria e confeitaria, com predominância de revenda, enquanto sua atividade secundária compreende a fabricação de produtos de padaria e confeitaria com predominância de produção própria.  A organização desempenha papel relevante na comunidade local ao comercializar produtos de consumo cotidiano e atender moradores da Vila Ré e de regiões próximas. A equipe é composta por 12 pessoas, sendo 2 sócios e 10 funcionários responsáveis pela operação diária, e a padaria atende em média 200 clientes por dia, além de aproximadamente 50 pedidos de delivery diários. Seu porte é adequado à realização de uma pesquisa acadêmica, pois permite observar diretamente processos como atendimento ao público, vendas, produção, controle de estoque, compras e organização administrativa. De acordo com o levantamento realizado junto à gestão, os principais problemas identificados na Padaria Delícia II estão relacionados ao controle de caixa e ao controle de clientes. A empresa não realiza um controle sistemático de entrada e saída de clientes, e o acompanhamento do consumo apresenta falhas, havendo casos em que clientes se retiram sem efetuar o pagamento, o que gera prejuízo financeiro. Esse controle é feito majoritariamente de forma verbal, com uso pontual de fichas de papel, e o controle de estoque também é realizado de maneira totalmente manual. Essa situação caracteriza a crise operacional da empresa e evidencia a necessidade de um sistema que centralize e padronize o controle de caixa, de clientes e de estoque.  A empresa apresenta problemas de organização que podem ser solucionados ou reduzidos mediante a implementação de ferramentas de gestão mais eficientes. Dessa forma, constitui um caso prático para analisar processos existentes e desenvolver soluções aplicáveis à sua rotina operacional.  A existência da organização pode ser comprovada pelo cadastro empresarial ativo, pelos diretórios comerciais e pelo perfil público da padaria no Instagram. Os cadastros consultados informam como administradores Francisco Almir Pereira e José Aldir das Chagas, além do telefone (11) 2958-2840 e do e-mail pyoner@ig.com.br. |
| **Fontes consultadas e verificadas** | A IA citou somente as fontes fornecidas. Isso se mostra no exemplo: “De acordo com o levantamento realizado junto à gestão”. |  
| **Trechos rejeitados ou corrigidos** | Foi adicionada uma justificativa em relação a escolha da empresa e o local da entrevista, na qual estava faltando para ficar completa e correta ao modelo condizente. Além de ser listado os problemas de forma mais explícita.|  
| **Justificativa da escolha final** | Foi mantida  a resposta sugerida, somente adicionando informações. A forma escrita foi bem avaliada e compreendida pelo grupo, sendo assim, não havendo alterações do que foi entregue pela IA. |  
| **Reflexão crítica** | Não houveram vieses, foram fornecidos os materiais necessários para a atividade desejada. |

| Item | O que registrar |
|------|------------------|
| **Ferramenta e etapa** | Gemini foi usado para revisão de requisitos e regras de negócio. |  
| **Motivação** | Havia uma grande quantidade de requisitos, o grupo pensou na possibilidade de existir algum que poderia ser excluído ou editado. |  
| **Prompt(s) utilizados** | Complemente, adicione ou exclua os requisitos funcionais e não funcionais de acordo com tudo que te mandei e com o arquivo texto.txt. |  
| **Resposta recebida** | O que é redundante e pode sair: * **RN13 (Acesso por turno)** e **RN14 (Horário de funcionamento contínuo)** não são realmente regras de negócio no sentido de modelagem — são só repetições do que já está no RF21/RNF04 (acesso por turno) e no RNF02 e na Introdução (horário de funcionamento). Manter os dois só em Section 3 é mais limpo; aqui eles só ocupam espaço sem adicionar nada nas restrições organizacionais.  * A nota "(era RN11 na versão anterior)" é resíduo de edição, não é conteúdo do trabalho.   * A seção final "O que mudou em relação à lista original" também não corresponde a nenhuma seção pedida pelo professor — é só o changelog da conversa. Isso deixa a 4.2 só com as duas restrições que são genuinamente legais/estatutárias (documentação regulatória e LGPD), que são exemplos muito mais fortes.
| **Fontes consultadas e verificadas** | RF01: Cadastro de produtos; RF02: Controle de categorias; RF03: Registro de vendas; RF04: Múltiplas formas de pagamento; RF05: Emissão de comprovantes; RF06: Controle automático de estoque; RF07: Alerta de estoque mínimo; RF08: Controle de validade; RF09: Controle de produção; RF10: Ficha técnica dos produtos; RF11: Controle de matéria-prima; RF12: Cadastro de fornecedores; RF13: Registro de compras; RF14: Cadastro de clientes; RF15: Programa de fidelidade; RF16: Pedidos antecipados; RF17: Encomendas personalizadas; RF18: Controle de caixa; RF19: Registro de sangrias e suprimentos; RF20: Cadastro de funcionários; RF21: Controle de permissões por perfil e turno; RF22: Cancelamentos e devoluções; RF23: Relatórios gerenciais; RF24: Dashboard operacional; RF25: Pesquisa e filtros; RF26: Histórico de alterações; RF27: Anexação de documentos e laudos regulatórios; RF28: Controle de consumo por comanda; RF29: Integração com WhatsApp. RNF01: Desempenho; RNF02: Disponibilidade; RNF03: Segurança; RNF04: Controle de acesso; RNF05: Integridade dos dados; RNF06: Backup; RNF07: Recuperação de dados; RNF08: Usabilidade; RNF09: Responsividade; RNF10: Compatibilidade com navegadores; RNF11: Escalabilidade do sistema; RNF12: Manutenibilidade; RNF13: Auditoria; RNF14: Privacidade e conformidade com a LGPD; RNF15: Confiabilidade; RNF16: Recuperação após falhas; RNF17: Facilidade de aprendizado; RNF18: Suporte a usuários simultâneos; RNF19: Implantação híbrida. RN01: Proibição da venda de produtos vencidos; RN02: Bloqueio de vendas acima do estoque disponível, salvo autorização; RN03: Autorização para descontos acima do limite estabelecido; RN04: Autorização para cancelamentos após o fechamento do caixa; RN05: Antecedência mínima para encomendas; RN06: Registro da produção antes da disponibilização para venda; RN07: Conferência entre o caixa esperado e o valor informado; RN08: Liberação de pontos de fidelidade após confirmação do pagamento; RN09: Proibição de encerramento de atendimento com comanda em aberto sem pagamento; RN10: Prioridade de alertas para produtos essenciais com estoque baixo; RN11: Manutenção da documentação regulatória exigida por lei; RN12: Proteção dos dados pessoais conforme a LGPD. |  
| **Trechos rejeitados ou corrigidos** | Adicionado: RF27, RF28, RF29, RN10, RN11, RN12, RN15 e RN16 com mais detalhamento. |  
| **Justificativa da escolha final** | Mantiveram-se partes do que foi retornado. Mas houve a necessidade de adicionar mais informações pelos levantamentos que foram feitos em grupo com o gerente do estabelecimento|  
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
| **Fontes consultadas e verificadas** | Nenhuma fonte externa. O grupo conferiu o texto com o dicionário de dados (seção 5), o DER (seção 7) e os números da entrevista (200 clientes/dia, 50 deliveries, 12 usuários, 19 entidades).|
| **Trechos rejeitados ou corrigidos** | Foram mantidas apenas as justificativas que correspondiam ao raciocínio real do grupo, pois a IA às vezes tratava como deliberadas escolhas feitas por conveniência. As limitações da seção 8.6 foram validadas pelo grupo.|
| **Justificativa da escolha final** | Foi mantida a versão condensada, por ser proporcional ao peso da seção e preservar a comparação com as alternativas. As decisões de modelagem foram do grupo, não da IA.|
| **Reflexão crítica** | A IA tende a apresentar toda decisão como intencional e bem fundamentada, o que gera texto convincente, mas nem sempre fiel ao processo real. Por isso, o grupo revisou cada justificativa. Não houve alucinação, pois o modelo completo foi fornecido, e não foram identificados vieses.|

| Item | O que registrar |
|------|------------------|
| **Ferramenta e etapa** | Claude foi usado na etapa de escrita da seção 6 (Modelagem Conceitual).|
| **Motivação** | Como o modelo ficou com muitas entidades (19 no total) e vários relacionamentos, ficava difícil escrever tudo em texto corrido sem esquecer nada ou repetir informação. Usamos a IA para ajudar a organizar esse conteúdo em tópicos (entidades, atributos, relacionamentos e restrições), já que ela consegue lidar melhor com uma quantidade grande de dados do que a gente escrevendo manualmente. |
| **Prompt(s) utilizados** | Poderia listar todos com base no arquivo enviado? Preciso separar em entidades, atributos, relacionamentos (com cardinalidade) e restrições, seguindo o que já está no dicionário de dados que te mandei antes. |
| **Resposta recebida** | A modelagem conceitual foi desenvolvida a partir dos principais processos e requisitos identificados na Padaria Delícia II, buscando representar as informações necessárias para o controle de produtos, estoque, produção, compras, vendas, clientes e operações de caixa. **6.1 Entidades reconhecidas:** foram listadas as 19 entidades do modelo — CATEGORIA, PRODUTO, MATERIA_PRIMA, ITEM_FICHA_TECNICA, DOCUMENTO_REGULATORIO, FORNECEDOR, COMPRA, ITEM_COMPRA, PRODUCAO, CLIENTE, FUNCIONARIO, COMANDA, ITEM_COMANDA, VENDA, ITEM_VENDA, CANCELAMENTO_DEVOLUCAO, CAIXA, MOVIMENTACAO_CAIXA e LOG_AUDITORIA — cada uma com uma frase curta explicando sua função (por exemplo, a entidade COMANDA existe justamente para resolver o problema real da padaria, que é cliente saindo sem pagar; e CAIXA representa cada sessão, da abertura até a conferência final). **6.2 Atributos e classificações:** a resposta manteve a mesma convenção de prefixos que já estava no dicionário de dados (ID_ para chave primária, NM_ para nome, CD_ para código, DT_ para data, QT_ para quantidade, VL_ para valor monetário, TP_ para tipo/categoria e DS_ para descrição). Os atributos foram detalhados para as entidades principais — CATEGORIA, PRODUTO, MATERIA_PRIMA, CLIENTE, FUNCIONARIO, VENDA e CAIXA — e para as demais entidades foi indicado que os atributos completos estão no Dicionário de Dados Conceitual, para não duplicar informação. **6.3 Relacionamentos pertinentes:** foram listados 21 relacionamentos com suas cardinalidades. O ponto que achamos mais importante de entender (e não só copiar) foi o relacionamento N:M entre PRODUTO e MATERIA_PRIMA, que precisou ser resolvido com uma entidade associativa (ITEM_FICHA_TECNICA), porque um relacionamento N:M não pode ser implementado diretamente em um banco relacional. Também aparecem os vínculos opcionais, como CLIENTE — COMANDA e CLIENTE — VENDA (o cliente pode não se identificar) e o relacionamento 1:1 opcional entre COMANDA e VENDA, que representa o momento em que uma comanda aberta no salão se transforma em uma venda fechada. **6.4 Restrições e políticas organizacionais:** foram reunidas as regras de negócio que o modelo precisa respeitar, como não deixar vender produto vencido, bloquear venda acima do estoque, exigir autorização para desconto alto ou para cancelamento depois do caixa fechado, dar prioridade nos alertas de estoque mínimo para produtos essenciais (tipo o pão francês), controlar o acesso por perfil/turno de funcionário, manter log de auditoria nas operações mais sensíveis e seguir a LGPD no tratamento dos dados de cliente e funcionário. |
| **Fontes consultadas e verificadas** | A IA usou apenas o que eu mandei: o dicionário de dados, o levantamento de requisitos e as informações que vieram da entrevista com a dono/gestão da padaria. Não foi citada nenhuma fonte externa. |
| **Justificativa da escolha final** | Decidimos manter o texto porque ele bateu com o que já tínhamos definido no dicionário de dados e no DER, e porque a explicação de cada relacionamento ajudou a gente a confirmar que o modelo fazia sentido com a realidade da padaria (principalmente a parte de comanda/venda, que foi a que mais discutimos como grupo). |
| **Reflexão crítica** | O principal ponto identificado foi a tendência da IA a apresentar todas as entidades e relacionamentos com o mesmo grau de certeza, sem distinguir o que já estava consolidado no dicionário de dados do que exigiria validação adicional, por isso o grupo revisou cada cardinalidade e atributo, conferindo se correspondiam de fato ao levantamento feito com a gestão da padaria. A qualidade da resposta dependeu totalmente do material fornecido: como o dicionário de dados e os requisitos já estavam completos, não houve alucinação de entidades ou regras inexistentes, a IA apenas organizou o que o grupo já havia definido. Não foram identificados vieses ou generalizações incorretas sobre o segmento de padarias. |
