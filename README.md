**Plataforma de gestão de recarga de veículos elétricos para condomínios**
Enterprise Challenge 2026 · Parceria GoodWe × FIAP · Energy Innovation Lab

---

## Equipe

| Integrante | RM |
| --- | --- |
| [PREENCHER] | [PREENCHER] |
| [PREENCHER] | [PREENCHER] |
| [PREENCHER] | [PREENCHER] |
| [PREENCHER] | [PREENCHER] |
| [PREENCHER] | [PREENCHER] |

**Equipe:** [PREENCHER NOME DA EQUIPE]

---

## Sumário

1. [O problema e o contexto]()
2. [Frente 1 — Contexto e Problema]
3. [Frente 2 — Base Regulatória e Técnica]
4. [Frente 3 — Arquitetura e IA]()
5. [Arquitetura da solução]()
6. [Modelo de rateio]()
7. [Papel da IA na solução]()
8. [Plano da Sprint 02]()
9. [Referências]()

---

## 1. O problema e o contexto

> **Como transformar sessões de recarga de veículos elétricos em uma infraestrutura compartilhada em dados estruturados, rateio justo e inteligência acionável?**

Com o crescimento acelerado da frota de veículos elétricos no Brasil, os condomínios residenciais enfrentam um desafio novo: disponibilizar infraestrutura de carregamento de forma compartilhada, organizada e financeiramente justa entre os moradores. Instalar carregadores não resolve, por si só, o controle de acesso, a medição individualizada do consumo nem o rateio dos custos de energia.

O **EV ChargeOps** é uma plataforma de gestão de recarga voltada a condomínios, desenvolvida em parceria com a GoodWe. A solução integra o protocolo aberto **OCPP** para comunicação com os carregadores, considera o enquadramento regulatório da **ANEEL** e apoia-se no hardware **GoodWe HCA G2**, oferecendo um modelo de faturamento e rateio que distribui os custos de forma transparente entre os usuários.

O ponto de partida é simples: por que gerir a recarga de veículos elétricos em um condomínio é difícil, e o que exatamente o EV ChargeOps precisa resolver? O núcleo do problema está em **por onde a energia é medida**. Em uma casa, o carregador fica atrás do medidor do próprio morador. No condomínio, o ponto de recarga costuma ser alimentado pela rede das áreas comuns — o mesmo relógio que mede elevadores, iluminação de garagem e bombas —, cuja energia entra no rateio condominial e é dividida entre todos. O efeito é imediato e injusto: quem não tem carro elétrico passa a subsidiar a recarga de quem tem. Daí nasce o problema central: **medir e atribuir a cada usuário exatamente o que ele consumiu**, transformando uma despesa coletiva e opaca em uma cobrança individual e rastreável.

### Opções de aprofundamento escolhidas

| Frente | Itens obrigatórios | Aprofundamento escolhido |
| --- | --- | --- |
| **Frente 1** | Infraestrutura compartilhada, sessão de recarga, modelos de negócio | **A** (análise de mercado), **B** (pesquisa de campo) e **C** (análise de dados públicos) |
| **Frente 2** | RN ANEEL 1.000/2021, interfaces HCA G2, API SEMS | **A** (mapeamento regulatório completo) e **B** (exploração da plataforma SEMS) |
| **Frente 3** | Camadas, fluxo de dados, modelo de rateio | [DEFINIR — em andamento] |

---

## 2. Frente 1 — Contexto e Problema

### 2.1 Infraestrutura de recarga compartilhada e desafios de gestão

Chamamos de **infraestrutura de recarga compartilhada** o conjunto de pontos de recarga instalados em local de uso coletivo — condomínio, edifício corporativo ou campus — em que mais de um usuário utiliza o mesmo equipamento ou, no mínimo, a mesma rede elétrica do empreendimento.

Três cenários de vaga, com implicações operacionais distintas:

- **Vaga pessoal:** carregador exclusivo, ligado à unidade do morador. Cobrança simples (vai direto à conta de luz dele), mas é o que mais consome capacidade elétrica individual.
- **Vaga rotativa:** um mesmo ponto usado por vários condôminos em horários diferentes. A medição individual é indispensável.
- **Vaga compartilhada:** ponto coletivo em área comum, com vários usuários potenciais. Exige um sistema completo de autenticação, medição, rateio e agendamento.

Os cenários **rotativo e compartilhado** são o território do EV ChargeOps. Os desafios operacionais agrupam-se em cinco eixos.

**a) Capacidade elétrica e risco de sobrecarga.** O pico de carga dos elétricos tende a coincidir com o pico residencial (fim de tarde/início da noite), sobrecarregando o quadro, provocando flutuações de tensão e, em casos extremos, desarmando disjuntores — com risco documentado de superaquecimento e incêndio. A resposta de mercado é a **gestão de carga (load management)**: na estática, cada carregador recebe uma fatia fixa de potência; na **dinâmica (DLB, dynamic load balancing)**, o sistema monitora o consumo total em tempo real e redistribui a potência entre os carregadores ativos. A gestão dinâmica permite instalar muito mais pontos dentro da mesma capacidade elétrica, evitando o upgrade caro da entrada de energia. Para prédios antigos, o EV ChargeOps combina três alavancas: **balanceamento dinâmico**, **agendamento inteligente** (espalha recargas pela madrugada) e **preço por horário** (incentivo concreto para carregar fora de ponta).

**b) Individualização do consumo e cobrança justa.** Sem medição individual, a energia da recarga é diluída na taxa condominial — fonte garantida de conflito em assembleia. Quanto à autenticação, o caminho mais comum (cartão RFID por usuário) esbarra em uma limitação confirmada pela própria GoodWe: o HCA G2 admite no máximo dez cartões por equipamento, o que inviabiliza um cartão por condômino em vaga compartilhada. Por isso o EV ChargeOps adota **autenticação pelo aplicativo**: o morador se identifica no app, que libera a recarga e atribui a sessão à unidade correta, sem depender de cartão físico por pessoa.

**c) Segurança e conformidade normativa.** A instalação envolve engenharia elétrica, dimensionamento de carga e responsabilidade técnica formal (ART). Normas aplicáveis: **ABNT NBR 5410**, **NBR 17019**, **NBR IEC 61851-1**, **NR-10** e instruções técnicas do Corpo de Bombeiros (ex.: IT-41, em São Paulo). O foco em **recarga AC** (wallboxes) é o que torna a solução viável dentro da capacidade elétrica de um condomínio comum.

**d) Disponibilidade, filas e agendamento.** Com poucos pontos para muitos interessados, agendamento e regras de prioridade evitam disputas. Sem gestão, a probabilidade de fila no pico cresce rapidamente com a frota; com balanceamento e agendamento, cai a níveis quase desprezíveis, porque boa parte das recargas migra para a madrugada.

**e) Lacunas regulatórias e responsabilidade do síndico.** Legislar sobre direito condominial e civil é competência privativa da União (art. 22, I, da Constituição) — o que explica por que leis municipais impositivas têm sido derrubadas. A discussão de quórum para aprovar a instalação ainda não convergiu nos tribunais (aquisição de equipamento, obra útil ou obra em área comum). A jurisprudência recente é consistente: o direito individual de instalar não se sobrepõe à segurança coletiva nem dispensa o rito de assembleia.

### 2.2 Dados de mercado no Brasil

- **Vendas recordes:** 2025 fechou com 223.912 veículos eletrificados vendidos (+26% sobre 2024). No 1º bimestre de 2026, 48.591 emplacamentos (+90% sobre o mesmo período de 2025).
- **Frota acumulada:** ~645,4 mil eletrificados (jan/2012 a jan/2026, critério ABVE); São Paulo lidera com ~181 mil.
- **Infraestrutura insuficiente:** 21.061 pontos públicos e semipúblicos em fev/2026 (+42% em 12 meses), relação de ~19,6 veículos/ponto contra a meta setorial de ~10:1.
- **Projeção (McKinsey):** frota de 1,4 milhão de elétricos até 2030, exigindo ~580 mil carregadores — ~490 mil residenciais/de frota e apenas ~90 mil públicos. O esforço de eletrificação concentra-se na recarga privada e compartilhada.

### 2.3 Aprofundamento A — Análise de mercado

Mapeamento de cinco soluções já em operação, do ponto de vista de um condomínio brasileiro:

| Solução | Origem | Força | Limitação para o contexto BR |
| --- | --- | --- | --- |
| **Zaptec Pro** | Noruega | Balanceamento dinâmico de carga e fases (patenteado); até 22 kW AC | Hardware premium europeu; rateio e tarifa por horário dependem do meio de pagamento acoplado |
| **Wallbox Pulsar Plus** | Espanha | Power Boost + Dynamic Power Sharing; OCPP 1.6J | Fabricante de hardware; rateio condominial e tarifação não são o núcleo |
| **ChargePoint** | EUA | Plataforma de rede em nuvem, escala por software | Assinatura amarrada ao hardware; sem presença/aderência regulatória no Brasil |
| **NeoCharge** | Brasil | Plataforma para condomínios; OCPP; fala o contexto local | Concentra-se em monitoramento; pouca inteligência de rateio |
| **EMEL / LEVE** | Portugal | Recarga pública municipal interoperável (MOBI.E) | Recarga pública, não gestão privada de condomínio |

**Conclusão.** Balanceamento de carga, medição individual, app e OCPP já são padrão de mercado. Os campos menos cobertos — **rateio condominial automatizado**, **tarifa por horário** e **aderência ao contexto regulatório brasileiro** — são exatamente onde o EV ChargeOps se posiciona: uma **plataforma de gestão** (não um fabricante) que une o padrão de mercado a três diferenciais: rateio justo com tarifa por horário inspirada na Tarifa Branca, lógica de assinatura com acerto de contas e IA como motor de previsão e justiça da cobrança.

### 2.4 Aprofundamento B — Pesquisa de campo

Questionário aplicado a moradores de um condomínio residencial real, com **78 respostas**. Por ser um único condomínio, os números têm valor indicativo, não estatístico. Principais achados:

| Achado | % | Implicação para o produto |
| --- | --- | --- |
| Têm/pretendem/considerariam um EV nos próximos anos | 88% | Demanda reprimida em ambiente *greenfield* |
| Condomínio sem nenhum ponto de recarga hoje | 100% | Sem sistema legado a substituir |
| Consideram justo o modelo atual (relógio individual) | 99% | Cultura já associa justiça a medição individual |
| Rejeitam ratear a recarga de vizinhos | 96% | Medição individual + reembolso ao condomínio são inegociáveis |
| Preferem pagar por kWh consumido (vs. mensalidade fixa) | 91% | Cobrança ancorada no consumo real |
| Aprovariam em assembleia com cobrança individual | 96% | A cobrança justa destrava o quórum |

No subgrupo de proprietários de EV (2 respondentes, leitura qualitativa), 100% topariam tarifa por horário, 100% já enfrentaram fila/conflito e 100% usariam um app — **desde que o agendamento seja automático**. O achado central: a barreira à recarga fora de ponta não é o preço, é o esforço manual. Isso transforma o **agendamento inteligente** de diferencial técnico em **requisito de adoção**.

### 2.5 Aprofundamento C — Análise de dados do mercado de recarga

A partir de dados setoriais (ABVE, Tupi Mobilidade, NeoCharge/Senatran, McKinsey), responde-se: a infraestrutura brasileira é suficiente para a frota projetada?

- **Frota:** entre dez/2024 e jun/2025, os eletrificados saltaram de 374.423 para 481.284 unidades (+28% em seis meses). Os segmentos plug-in crescem mais rápido (BEV +33,8%, PHEV +32,3%).
- **Infraestrutura:** carregadores rápidos/ultrarrápidos (DC) saltaram de 2.430 para 6.479 (+167%), já 31% da base. Um ponto DC atende ~20+ carros/dia contra ~2–3 de um ponto AC lento.
- **Suficiência:** relação atual de 19,6:1, abaixo da meta de 10:1. No ritmo atual (+42%/ano), a meta não é alcançada até 2030 — seria preciso ~60%/ano.

**Elo com o projeto:** os ~490 mil carregadores residenciais e de frota projetados para 2030 superam em muito os ~90 mil públicos. A recarga domiciliar e condominial é tanto o que sustenta a frota hoje quanto o segmento que mais crescerá — o território do EV ChargeOps.

### 2.6 Da dor à proposta: modelo de gestão e de preços

Um bom sistema de recarga condominial precisa resolver, ao mesmo tempo, três coisas que normalmente entram em conflito: **proteger a rede**, **ser justo** e **ser atrativo**.

- **Camada técnica:** gestão dinâmica de carga, para atender mais carros com a mesma entrada de energia.
- **Camada de preço:** preços por faixa de horário espelhando a **Tarifa Branca** da ANEEL (ponta / intermediário / fora de ponta), com cashback fora de ponta, tarifa de ociosidade e opção de recarga rápida paga.
- **Camada de cobrança:** **assinatura com acerto de contas** — o morador paga uma estimativa, o sistema mede kWh a kWh com preço por horário e, no fechamento, ajusta o saldo. A energia é reembolsada ao condomínio via taxa condominial, de modo que quem não tem EV não paga nada pela recarga de quem tem.

---

## 3. Frente 2 — Base Regulatória e Técnica

### 3.1 Seis camadas normativas da recarga compartilhada

A recarga compartilhada com geração solar não é governada por uma única lei, mas por seis planos normativos:

| Camada | Trata de | Principais normas |
| --- | --- | --- |
| **1. Setorial (energia)** | Cobrança da recarga e abatimento solar | REN ANEEL 1.000/2021, 1.059/2023; Lei 14.300/2022 |
| **2. Técnico-normativa** | Segurança e interoperabilidade da instalação | NBR 17019, 5410/5419, IEC 61851, IEC 62196, NR-10 |
| **3. Industrial (fomento)** | Diretrizes e incentivos de mercado | Lei 14.902/2024 (Mover); Decreto 12.435/2025 |
| **4. Tributária** | Impostos sobre a recarga | LC 116/2003, Súmula 391 STJ, EC 132/2023 (IBS/CBS), LC 214/2025 |
| **5. Condominial (civil)** | Permissão, deveres e quem decide | Código Civil; Lei SP 18.403/2026; PL 158/2025 |
| **6. Proteção de dados** | Sessão por usuário e dados pessoais | LGPD (Lei 13.709/2018) |

**Camada 1 — Setorial.** A REN 1.000/2021 dá à recarga um capítulo próprio (Cap. V) e consagra a **liberdade de exploração comercial**: qualquer interessado pode oferecer recarga, inclusive cobrando, sem concessão ou autorização da ANEEL. O EV ChargeOps **não compra nem revende energia** — presta um serviço de recarga, atribui a cada morador o consumo que foi dele e cobra por esse serviço, distinguindo na fatura a parcela da rede (tarifa cheia) da parcela compensada pelo sol via **SCEE** (Lei 14.300/2022 + REN 1.059/2023).

**Camada 2 — Técnico-normativa.** Carregar um carro não é ligar um eletrodoméstico: a HCA G2 entrega de 7 a 22 kW de forma contínua. A NBR IEC 61851 classifica a recarga em modos, e o relevante para o condomínio é o **Modo 3** (estação fixa AC, conector Type 2, comunicação de controle) — exatamente o que permite medir cada sessão e regular a potência. A instalação exige circuito dedicado, proteção contra corrente residual, dimensionamento de fiação e aterramento. O **controle dinâmico de carga** da HCA G2 deixa de ser diferencial e passa a ser requisito estrutural.

**Camada 3 — Industrial.** O Programa **Mover** estimula a demanda por veículos, mas faz pouco pela oferta de infraestrutura de recarga — o gargalo que trava a adoção em condomínios. Esse vácuo é a oportunidade de negócio que o EV ChargeOps ocupa.

**Camada 4 — Tributária.** A camada mais instável: disputa entre **ICMS** (Estados, ~18–21%, energia como mercadoria) e **ISS** (Municípios, ~2–5%, recarga como serviço), com risco de bitributação. A reforma (EC 132/2023) cria o **IBS/CBS**, que tende a unificar a questão; a LC 214/2025 exclui da base a energia compensada de geração distribuída até 1 MW. **Decisão de arquitetura:** o imposto não pode ser constante embutida no código — a fatura precisa tratar o tributo como **parâmetro configurável** e excluir automaticamente da base a energia compensada.

**Camada 5 — Condominial.** O Código Civil (art. 1.340) é a base para cobrar a recarga de quem efetivamente recarregou. A Lei SP 18.403/2026 impede que a convenção proíba a instalação sem justificativa técnica fundamentada. Especialistas consideram o rateio estimado inviável, recomendando a medição por vaga com cobrança automática — o que valida o núcleo de **medição individual por sessão** do projeto.

**Camada 6 — Proteção de dados.** Ao ligar cada sessão a uma pessoa, o sistema trata dados pessoais e cai sob a LGPD. A base legal mais adequada é a execução de contrato (art. 7º, V), reforçada pelo legítimo interesse (IX). O art. 20 dá ao titular o direito de revisão de decisões automatizadas — em rota de colisão com qualquer IA "caixa-preta". **Decisão:** privacy by design, base legal documentada em contrato e **memória de cálculo auditável e explicável célula a célula**.

**Contraponto internacional.** A UE (**AFIR**, Reg. 2023/1804) já exige transparência de preço antes da sessão, pagamento sem cadastro e abertura de dados; os EUA (**NEVI**) condicionam financiamento a padrões abertos (OCPP). No Brasil isso ainda não é obrigatório — e é nesse silêncio que mora a oportunidade. Por isso o EV ChargeOps adota **arquitetura aberta por opção**, antecipando o que a regulação estrangeira já exige.

### 3.2 Carregador GoodWe HCA G2 — interfaces

A série HCA G2 (modelos GW7K/GW11K/GW22K-HCA-20) troca dados por cinco interfaces:

| Interface | Função no equipamento | Papel no EV ChargeOps |
| --- | --- | --- |
| **RS-485** | Comunicação com inversor (A1/B1) e medidor MID (A2/B2) | Medição de energia por ponto (base do rateio) + controle dinâmico de carga |
| **LAN** | Conexão cabeada com roteador/nuvem (SEMS) | Telemetria estável das sessões até o back-end |
| **Wi-Fi** | Conexão sem fio (2,4 GHz) com roteador/nuvem | Alternativa de telemetria (decisão de instalação) |
| **Bluetooth (BLE)** | Configuração local pelo app SolarGo | Comissionamento e manutenção; fora do fluxo de cobrança |
| **RFID** | Identificação do usuário (13,56 MHz; até 10 cartões) | Atribuição da sessão ao usuário (base da identificação) |

O **limite de dez cartões por carregador** não escala em condomínios com dezenas de moradores. A plataforma gerencia a **identidade do usuário em camada própria** no back-end, vinculando credenciais a pessoas e unidades, sem depender do firmware.

### 3.3 Plataforma SEMS+ — dados expostos

Inspeção direta do ambiente **LAB FIAP Eco Smart Home** (sistema GoodWe compartilhado, usado como gêmeo do cenário condominial). Como a equipe **não possui credenciais de API**, documenta-se o que a plataforma expõe — campos que a API serviria de forma equivalente, ficando a especificação dos endpoints para a Sprint 02.

Dados expostos por sessão e seu uso:

| Dado exposto | Onde aparece | Uso no EV ChargeOps |
| --- | --- | --- |
| Estado operacional | Lista de dispositivos / tela do carregador | Disponibilidade e monitoramento em tempo real |
| Potência instantânea (kW) | Tela do carregador / Energy Flow | Gerenciamento de demanda e balanceamento |
| Duração da sessão | Last Charge / Charging Record | Composição da sessão para a fatura |
| Energia por sessão (kWh) | Last Charge / registro de sessões | **Base do rateio por consumo individual** |
| **Origem: solar × rede por sessão** | Aba Charge Monitoring (Green/Grid/Charged) | **Exclusão da energia compensada (REN 1.059) e rateio justo** |
| Identificação do cartão (Card ID) | Cartão Last Charge | Atribuição da sessão a um responsável |
| Porta de recarga | Cartão Last Charge | Rastreio do ponto físico utilizado |
| Curva de potência | Aba Operation Monitoring | Detecção de picos e ociosidade (IA) |

**Achado decisivo:** a aba *Charge Monitoring* decompõe cada sessão em **Green Charging** (solar), **Grid Charging** (rede) e **Charged Energy** (total). A obrigação regulatória de não tratar energia compensada como consumo da rede já existe como **dado nativo** na plataforma — uma exigência da Camada 1 que se converte em campo pronto para alimentar o rateio justo.

**Decisão de arquitetura.** O EV ChargeOps consome a SEMS como fonte de telemetria, energia por sessão e separação solar × rede, mas mantém em camada própria o que a plataforma não faz: **identidade do usuário em escala**, **memória de cálculo da fatura** (tributo parametrizável + exclusão automática da energia compensada) e **trilha auditável** exigida pela LGPD. Como a estação aparece por vezes *Offline*, a arquitetura deve tolerar lacunas de telemetria com **bufferização local e reconciliação posterior**.

---

## 4. Frente 3 — Arquitetura e IA

## 5. Arquitetura da solução



## 6. Modelo de rateio

## 7. Papel da IA na solução

## 8. Plano da Sprint 02

## 9. Referências

> Todas as fontes abaixo foram efetivamente consultadas. Lista consolidada das Frentes 1 e 2.

### Regulação e legislação

- ANEEL. **Resolução Normativa nº 1.000**, de 7 dez. 2021. <https://www2.aneel.gov.br/cedoc/ren20211000.html>
- ANEEL. **Resolução Normativa nº 1.059**, de 7 fev. 2023. <https://www2.aneel.gov.br/cedoc/ren20231059.html>
- ANEEL. **Resolução Normativa nº 819**, de 19 jun. 2018 (revogada). <https://www2.aneel.gov.br/cedoc/ren2018819.html>
- ANEEL. **Veículos Elétricos**. <https://www.gov.br/aneel/pt-br/assuntos/veiculos-eletricos>
- ANEEL. **Tarifa Branca: introdução**. <https://www.gov.br/aneel/pt-br/assuntos/tarifas/tarifa-branca/introducao>
- BRASIL. **Lei nº 14.300**, de 6 jan. 2022 (Marco Legal da GD).
- BRASIL. **Lei nº 14.902**, de 27 jun. 2024 (Programa Mover); **Decreto 12.435/2025**. <https://www.planalto.gov.br/ccivil_03/_Ato2023-2026/2024/Lei/L14902.htm>
- BRASIL. **Lei nº 10.406**, de 10 jan. 2002 (Código Civil — arts. 1.331, 1.336, 1.340, 1.341, 1.342).
- SÃO PAULO (Estado). **Lei nº 18.403**, de 18 fev. 2026.
- CÂMARA DOS DEPUTADOS. **PL 158/2025**. <https://www.camara.leg.br/noticias/1134716-projeto-regulamenta-instalacao-de-ponto-individual-de-recarga-de-carro-eletrico-em-condominio/>
- UNIÃO EUROPEIA. **Regulation (EU) 2023/1804 (AFIR)**. <https://transport.ec.europa.eu/transport-themes/clean-transport/alternative-fuels-sustainable-mobility-europe/alternative-fuels-infrastructure_en>

### Tributação

- CONJUR. A ascensão da mobilidade elétrica e o dilema tributário (ICMS ou ISS). <https://www.conjur.com.br/2025-jul-25/a-ascensao-da-mobilidade-eletrica-e-o-dilema-tributario-no-carregamento-de-veiculos-icms-ou-iss/>
- SEFAZ-SP. **Resposta a Consulta RC 30579/2024**. <https://legislacao.fazenda.sp.gov.br/Paginas/RC30579_2024.aspx>

### Normas técnicas

- ABNT. **NBR 17019:2022** — Alimentação de veículos elétricos.
- ABNT. **NBR 5410** / **NBR 5419** / **NBR IEC 61851** / **NBR IEC 62196**.
- MINISTÉRIO DO TRABALHO. **NR-10**.
- CANAL SOLAR. ABNT publica norma para instalação de carregadores. <https://canalsolar.com.br/abnt-publica-norma-para-instalacao-de-carregadores-de-veiculos-eletricos/>
- CELESC. **Instrução Normativa I-321.0043** — Estações de recarga. <https://www.celesc.com.br/arquivos/normas-tecnicas/padrao-entrada/I-321-0043-Estacoes-de-recarga-de-veiculos-eletricos.pdf>
- CORPO DE BOMBEIROS PM-SP. **IT-41** e **Portaria 003/970/2026** (SAVE).

### Hardware GoodWe e plataforma SEMS

- GOODWE. **HCA G2 Series AC Charger** (página oficial e datasheet). <https://en.goodwe.com/hca-g2>
- CANAL SOLAR. GoodWe HCA G2 — análise técnica. <https://canalsolar.com.br/en/goodwe-hca-g2-electric-vehicle-charging/>
- GOODWE. **SEMS Portal / SEMS+**. <https://semsplus.goodwe.com>
- GOODWE Technologies Co., Ltd. **Manual do usuário — Carregador CA, Série HCA (7 a 22 kW) G2**. Versão V1.5, 11 nov. 2025.

### Protocolos e interoperabilidade

- OPEN CHARGE ALLIANCE. **Open Charge Point Protocol (OCPP)** — visões 1.6 / 2.0.1; ISO 15118.
- NEOCHARGE. OCPP — Protocolo de Ponto de Carga Aberto. <https://www.neocharge.com.br/tudo-sobre/carregador-carro-eletrico/protocolo-ocpp>
- VOLTBRAS. Benefícios do Protocolo OCPP na gestão de eletropostos. <https://voltbras.com/uncategorized-pt/beneficios-do-protocolo-ocpp-na-gestao-de-eletropostos/>
- IEA. Regulation (EU) 2023/1804 — Alternative Fuels Infrastructure Regulation. <https://www.iea.org/policies/27375-regulation-eu-20231804-alternative-fuels-infrastructure-regulation>

### Dados de mercado e infraestrutura

- ABVE. Recarga pública rápida cresce 167% em 12 meses. <https://abve.org.br/recarga-publica-rapida-cresce-167-em-12-meses-e-ja-atinge-31-dos-21-mil-eletropostos-da-rede/>
- AUTOINDÚSTRIA. Já são 21 mil pontos de recarga no Brasil. <https://www.autoindustria.com.br/2026/03/04/ja-sao-21-mil-postos-de-recarga-de-carro-eletrico-no-brasil/>
- CANALVE. Frota de eletrificados ultrapassa 700 mil unidades. <https://canalve.com.br/frota-eletrificados-brasil-ultrapassa-700-mil-unidades/>
- CANALVE. Frota de eletrificados cresce 28% no primeiro semestre. <https://canalve.com.br/frota-eletrificados-brasil-cresce-28-primeiro-semestre/>
- McKINSEY & COMPANY. Recarregar para crescer. <https://www.mckinsey.com/br/our-insights/all-insights/recarregar-para-crescer>
- ELÉTRICOS BRASIL. Mapa de eletropostos e tipos de recarga. <https://www.eletricos.app/mapa-eletropostos>

### Condomínio e jurisprudência

- SINDICONET. Lei recarga carros elétricos condomínios SP 2026. <https://www.sindiconet.com.br/informese/lei-recarga-carros-eletricos-condominios-sp-2026-noticias-juridico>
- MIGALHAS. Moradora não pode instalar carregador sem aval do condomínio. <https://www.migalhas.com.br/quentes/444793/moradora-nao-pode-instalar-carregador-veicular-sem-aval-de-condominio>
- CONJUR. Instalação de carregador exige aprovação de assembleia. <https://www.conjur.com.br/2024-jan-18/instalacao-de-carregador-de-carro-eletrico-em-predio-exige-aprovacao-de-assembleia/>
- NICAIRA RODRIGUES ADVOCACIA. Carregador de carro elétrico em condomínio: guia para síndicos. <https://nicairarodriguesadvocacia.com.br/carregador-de-carro-eletrico-em-condominio-guia-completo-para-sindicos/>

### LGPD

- WAY2. LGPD no setor elétrico. <https://way2.com.br/blog/lgpd-no-setor-eletrico/>

### Dados primários

- PESQUISA DE CAMPO. Questionário a moradores de um condomínio residencial sobre recarga de veículos elétricos (78 respostas). Lisboa/São Paulo: elaboração própria, jun. 2026.
- INSPEÇÃO DA PLATAFORMA SEMS+ — ambiente LAB FIAP Eco Smart Home, 17 jun. 2026.
