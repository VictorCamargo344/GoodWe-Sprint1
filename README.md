# EV ChargeOps — Gestão Inteligente de Recarga Compartilhada em Condomínios
**Enterprise Challenge 2026 · Parceria GoodWe × FIAP · Sprint 01 — Pesquisa e Documentação**
---
## Equipe
| Integrante | RM |
| --- | --- |
| Eduardo Lopes da Silveira Mota | 563418 |
| Gabriel Luís de Lima Ramos | 568984 |
| Mayara Luisa Vicente Rosa | 571955 |
| Victor de Camargo Gomes | 568912 |
| Matheus Silveira | — |
---
## Sumário
1. [O problema e o contexto do desafio](#1-o-problema-e-o-contexto-do-desafio)
2. [Frente 1 — Contexto e Problema](#2-frente-1--contexto-e-problema)
3. [Frente 2 — Base Regulatória e Técnica](#3-frente-2--base-regulatória-e-técnica)
4. [Frente 3 — Arquitetura e IA](#4-frente-3--arquitetura-e-ia)
5. [Diagrama de arquitetura](#5-diagrama-de-arquitetura)
6. [Modelo de rateio](#6-modelo-de-rateio)
7. [O papel da IA na solução](#7-o-papel-da-ia-na-solução)
8. [Plano para a Sprint 02](#8-plano-para-a-sprint-02)
9. [Fontes consultadas](#9-fontes-consultadas)
---
## 1. O problema e o contexto do desafio
Com o crescimento acelerado da frota de veículos elétricos no Brasil, os condomínios residenciais enfrentam um novo desafio: como disponibilizar infraestrutura de recarga de forma compartilhada, organizada e financeiramente justa entre os moradores. A simples instalação de carregadores não resolve o controle de acesso, a medição individualizada de consumo nem o rateio dos custos de energia.
No cenário típico de condomínio, o ponto de recarga é alimentado pela rede das áreas comuns, o mesmo relógio que mede elevadores, iluminação e bombas. Essa energia entra no rateio condominial e é dividida entre todos na taxa mensal, fazendo com que **quem não tem carro elétrico acabe subsidiando a recarga de quem tem**. 
Daí nasce o problema central: medir e atribuir a cada usuário exatamente o que ele consumiu, transformando uma despesa coletiva e opaca em uma cobrança individual e rastreável.
O **EV ChargeOps** (Electric Vehicle Charge Operations) é a resposta a esse problema: uma plataforma de gestão de recarga de veículos elétricos voltada a condomínios, desenvolvida em parceria com a GoodWe. A solução apoia-se no hardware **GoodWe HCA G2** e comunica-se com ele pelo ecossistema nativo da GoodWe **Modbus TCP** na rede local e **API SEMS** na nuvem, considera o enquadramento regulatório da ANEEL e oferece um modelo de faturação e rateio que distribui os custos de forma transparente.
> **A pergunta que organiza o projeto:** como transformar sessões de recarga em uma infraestrutura compartilhada em dados estruturados, rateio justo e inteligência acionável, com IA como motor lógico?
**Decisões transversais do MVP** (definidas na Frente 3 e que orientam toda a solução):
- **Cenário de referência:** carregador rotativo em condomínio, atendendo múltiplos moradores em um mesmo ponto físico — não o cenário simplificado de uma vaga por unidade. É o cenário que o desafio propõe e que a infraestrutura real do laboratório FIAP (HCA G2 instalado na Unidade Aclimação) reproduz.
- **Comunicação:** ecossistema GoodWe (Modbus TCP localmente e API SEMS na nuvem), **sem OCPP** — o HCA G2 não o implementa. O OCPP fica registrado como vetor de evolução futura, não como compromisso arquitetural do MVP.
- **Identificação do morador:** por **reserva prévia no aplicativo**, e não por cadastro individual de cartão RFID no firmware, contornando o limite de dez cartões por equipamento e preservando a escalabilidade para condomínios com dezenas de unidades.
---
## 2. Frente 1 — Contexto e Problema
A Frente 1 percorre as camadas que se sobrepõem na prática condominial, infraestrutura elétrica, individualização do consumo, segurança, disponibilidade e moldura jurídica e consolida o problema em uma proposta de solução.
### 2.1 Itens obrigatórios
**Infraestrutura de recarga compartilhada e desafios de gestão.** A diferença essencial em relação à recarga doméstica está em por onde a energia é medida: em casa, o carregador fica atrás do medidor do próprio morador; no condomínio, costuma ser alimentado pela rede comum e diluído na taxa. Os desafios operacionais agrupam-se em cinco eixos: (a) **capacidade elétrica e risco de sobrecarga**, já que o pico de recarga coincide com o pico residencial; (b) **individualização do consumo e cobrança justa**; (c) **segurança e conformidade normativa** (ABNT NBR 5410, NBR 17019, NBR IEC 61851-1, NR-10 e instruções do Corpo de Bombeiros, com emissão de ART); (d) **disponibilidade, filas e agendamento**; e (e) **lacunas regulatórias e responsabilidade do síndico**.
**Como funciona uma sessão de recarga.** O foco do condomínio é a recarga em **corrente alternada (CA)**, em que o carregador (wallbox) entrega CA e o conversor interno do veículo faz a conversão para CC. É a recarga lenta/semirrápida viável dentro da capacidade elétrica de um prédio comum, ao contrário da recarga ultrarrápida (DC), cara e raramente cabível em ambiente residencial.
**Modelos de negócio.** Mapeamos os formatos praticados no Brasil e no mundo, gratuito, por kWh, por tempo, assinatura mensal e rateio condominial, analisando vantagens e limitações de cada um para o contexto condominial.
### 2.2 Opções de aprofundamento escolhidas
Foram desenvolvidas **as três opções** (A, B e C), formando um triângulo de evidências:
- **Opção A -  Análise de mercado.** Mapeamento de cinco soluções em operação: **Zaptec Pro** (Noruega), **Wallbox Pulsar Plus** (Espanha), **ChargePoint** (EUA), **NeoCharge** (Brasil) e **EMEL/LEVE** (Portugal, recarga pública municipal). Conclusão: balanceamento de carga, medição individual e app já são padrão de mercado; o espaço de diferenciação está no **rateio condominial automatizado, na tarifa por horário (inspirada na Tarifa Branca) e na aderência à regulação brasileira**, justamente onde o EV ChargeOps se posiciona.
- **Opção B - Pesquisa de campo.** Questionário aplicado a moradores de um condomínio real, com **78 respostas**. Achados-chave: 88% têm intenção de adquirir um EV e 100% afirmam não haver ponto de recarga hoje (cenário *greenfield*); 96% rejeitam ratear a recarga alheia e 99% valorizam medição individual; 91% preferem pagar por kWh consumido; 96% aprovariam a instalação em assembleia se a cobrança fosse individual; entre quem já tem EV, 100% aceitam tarifa por horário **desde que o agendamento seja automático**. A pesquisa transforma o agendamento inteligente de diferencial técnico em **requisito de adoção**.
- **Opção C - Análise de dados públicos.** Dimensionamento do mercado a partir de ABVE, Tupi Mobilidade, NeoCharge/Senatran e McKinsey: a frota de eletrificados cresce em ritmo próximo da duplicação anual (48.591 emplacamentos só no 1º bimestre de 2026, +90% sobre 2025), enquanto a rede de recarga avança ~42% ao ano. A projeção da McKinsey aponta ~580 mil carregadores até 2030, dos quais ~490 mil residenciais e de frota — confirmando que o esforço de eletrificação se concentra na **recarga privada e compartilhada**, o território do EV ChargeOps.
---
## 3. Frente 2 - Base Regulatória e Técnica
A Frente 2 mapeia o ambiente normativo e o equipamento, convertendo cada fragilidade jurídica em uma decisão de projeto.
### 3.1 Itens obrigatórios
**Seis camadas normativas.** A recarga compartilhada no Brasil é regulada por seis planos sobrepostos: **setorial/energia** (REN ANEEL 1.000/2021, 1.059/2023 e Lei 14.300/2022), **técnico-normativa** (ABNT/NBR e NR-10), **industrial/fomento** (Programa Mover - Lei 14.902/2024), **tributária** (ICMS × ISS, com transição para IBS/CBS), **condominial/civil** (Código Civil e Lei SP 18.403/2026) e **proteção de dados** (LGPD). O ponto-chave da camada setorial é que o EV ChargeOps **não revende energia**: presta um serviço de recarga, atribui a cada morador o consumo que foi dele e cobra por esse serviço ,  atividade comercial livre, sem necessidade de concessão.
**Carregador GoodWe HCA G2 - interfaces.** O equipamento (modelos GW7K, GW11K e GW22K-HCA-20; 7 a 22 kW AC) troca dados por cinco interfaces, cada uma com um papel na solução:
| Interface | Função no equipamento | Papel no EV ChargeOps |
| --- | --- | --- |
| **RS-485** | Comunicação com o inversor (A1/B1) e o medidor MID (A2/B2) | Medição de energia por ponto (base do rateio) e controle dinâmico de carga |
| **LAN** | Conexão cabeada com o roteador e a nuvem SEMS | Telemetria estável das sessões até o back-end |
| **Wi-Fi** | Conexão sem fio (2,4 GHz) com a nuvem | Alternativa de telemetria (decisão de instalação) |
| **Bluetooth (BLE)** | Configuração local pelo app SolarGo | Comissionamento e manutenção (fora do fluxo de cobrança) |
| **RFID** | Identificação do usuário (13,56 MHz; até 10 cartões) | Atribuição da sessão ao usuário — restrição de escala tratada na arquitetura |
**API GoodWe (SEMS Portal / SEMS+).** A inspeção direta do ambiente *LAB FIAP Eco Smart Home* documentou o que a plataforma expõe sobre o carregador: estado operacional, potência instantânea, duração e energia por sessão (kWh), **separação por origem solar × rede por sessão** (campos Green / Grid / Charged), Card ID, porta de recarga e curva de potência. O achado decisivo: a obrigação regulatória de não tratar energia solar compensada como consumo da rede **já existe como dado nativo** na plataforma.
### 3.2 Opções de aprofundamento escolhidas
Foram desenvolvidas **as três opções**:
- **Opção A - Mapeamento regulatório completo.** Análise das seis camadas, incluindo a correção tributária sobre a transição para IBS/CBS (LC 214/2025, art. 28, e Decreto 12.955/2026, art. 15) e a exclusão da energia compensada como parâmetro dependente da modalidade de geração, não como dedução automática.
- **Opção B - Exploração da API GoodWe.** Documentação dos dados expostos pela SEMS+ por sessão e no nível da planta (energia por origem, séries de potência, tarifas, eventos/alarmes, agregados e dados meteorológicos), com a decisão de arquitetura de tolerar lacunas de telemetria via bufferização local e reconciliação posterior.
- **Opção C - APIs complementares.** Mapeamento do contraponto internacional (AFIR na UE, NEVI nos EUA, ISO 15118 e OCPP) e de fontes externas (previsão meteorológica via INMET) que alimentam a inteligência da solução.
**Síntese da Frente 2 - requisitos de produto:** imposto parametrizável por município/estado e período; campo de energia compensada na fatura; compromisso com o Modo 3 e o balanceamento dinâmico de carga; medição individual por sessão; *privacy by design* com contrato controlador/operador; e fatura transparente e auditável.
---
## 4. Frente 3 - Arquitetura e IA
A Frente 3 traduz o diagnóstico das Frentes 1 e 2 em arquitetura concreta.
### 4.1 As quatro camadas da plataforma
- **Camada física**  o carregador GoodWe HCA G2, o inversor/bateria/medidor MID e um **gateway local** (Raspberry Pi ou ESP32) que lê os dados via Modbus TCP, armazena cada sessão de forma persistente e sincroniza com a nuvem quando a internet permite. O gateway é a garantia de que nenhuma sessão se perde por intermitência da SEMS.
- **Camada de conectividade** -  duas vias paralelas: a **via local** (gateway ↔ carregador via Modbus TCP) e a **via nuvem** (carregador → SEMS → back-end). Em operação normal, o back-end consome preferencialmente a SEMS (que já separa solar × rede); quando a SEMS falha, o gateway local prevalece como fonte da verdade.
- **Camada de aplicação** - o cérebro da solução, em **Python + FastAPI** com **PostgreSQL**. Três módulos: ingestão e reconciliação, regras de negócio (modelo de rateio) e inteligência artificial.
- **Camada de apresentação** - duas interfaces sobre os mesmos endpoints REST: o **app do morador** (reservas, alertas, histórico, memória de cálculo e fatura) e o **painel do gestor** (sessões em tempo real, gestão de reservas, configuração de tarifas e overhead, fila de anomalias e relatório de arrecadação).
### 4.2 Fluxo de dados (da sessão à fatura)
O dado percorre sete etapas: **(1)** reserva no app → **(2)** início da sessão (cartão compartilhado + captura de timestamp e Card ID) → **(3)** telemetria contínua (potência, energia, separação solar/rede) → **(4)** encerramento e classificação (concluída, encerrada pelo usuário ou interrompida por falha) → **(5)** atribuição ao morador pela reserva + **verificação de anomalia (IA)** → **(6)** cálculo da fatura com memória célula a célula → **(7)** **recomendação da próxima janela (IA)**.
### 4.3 Opções de aprofundamento escolhidas
- **Opção A - Benchmarking de modelos de rateio.** Comparação entre o **rateio por consumo real (kWh)** e a **cobrança por tempo/sessão com tarifa de ociosidade**, justificando o **modelo híbrido** adotado (detalhado na seção 6).
- **Opção B - Definição do papel da IA.** Duas abordagens estruturais, previsão de janela ótima e detecção de anomalias (detalhadas na seção 7 e no documento de apoio `Papel_da_IA.docx`).
---
## 5. Diagrama de arquitetura
A figura abaixo representa as quatro camadas da plataforma, as duas vias de conectividade, os módulos do back-end (incluindo a IA), a faixa de conformidade transversal e o fluxo do dado da sessão até a fatura.
![Diagrama de arquitetura do EV ChargeOps em quatro camadas, da camada física com o carregador GoodWe HCA G2 e o gateway local, passando pela conectividade (via local Modbus TCP e via nuvem SEMS), pela camada de aplicação com os módulos de ingestão, regras de negócio e IA, até a camada de apresentação com o app do morador e o painel do gestor.](./arquitetura.png)
*Figura 1 - Arquitetura da solução EV ChargeOps. Elaboração própria.*
---
## 6. Modelo de rateio
O modelo foi desenhado para satisfazer três princípios simultaneamente: **justiça** (cada morador paga apenas o que usou), **transparência** (memória de cálculo auditável célula a célula) e **parametrização** (faixas tarifárias, impostos e overheads definidos pela assembleia, não fixados no código).
### 6.1 Modelo escolhido: híbrido
A base é a **cobrança por consumo real de energia (kWh)** com fechamento mensal individual (justiça financeira do Modelo 1), acrescida de **regras de ociosidade/overstay** para evitar a ocupação indevida da vaga após o fim da recarga (eficiência operacional do Modelo 2). Essa combinação é a mais adequada a condomínios com muitos veículos elétricos e poucos pontos de recarga.
### 6.2 Composição da fatura individual
A fatura mensal de cada morador é a soma de todas as suas sessões válidas, com três parcelas por sessão:
| Variável | Critério de cálculo |
| --- | --- |
| **Energia verde** | kWh do campo *Green Charging* (SEMS) × preço Green da faixa horária da sessão |
| **Energia da rede** | kWh do campo *Grid Charging* (SEMS) × preço Grid da faixa horária da sessão |
| **Tempo de ociosidade** | Minutos após tolerância de 15 min do fim do fluxo de energia × taxa por minuto (assembleia) |
Sobre o subtotal incidem dois acréscimos: o **imposto** correspondente (ICMS, ISS ou IBS, conforme município e período da reforma tributária), destacado na fatura; e um **overhead operacional opcional**, percentual sobre o subtotal definido pela assembleia, que o condomínio pode zerar ou aplicar.
As **faixas horárias** (ponta, intermediário e fora de ponta) e seus preços espelham a lógica da Tarifa Branca da ANEEL e são definidas pela assembleia.
### 6.3 Reembolso ao condomínio
O condomínio paga à distribuidora a conta integral do mês. O valor arrecadado nas faturas individuais retorna ao caixa, **reembolsando exatamente o que foi gasto com a energia dos carros**. Resultado: quem não usa o carregador tem fatura zero (sem assinatura mínima), e a diluição na taxa condominial desaparece — o argumento que destrava a aprovação em assembleia deixa de ser promessa e passa a ser arquitetura.
### 6.4 Casos excepcionais
| Caso | Tratamento |
| --- | --- |
| **Sessão interrompida** | Paga-se apenas a energia entregue até a interrupção. Se a causa for falha do sistema (código de erro ou queda de comunicação), **não há cobrança**. A taxa de ociosidade não se aplica. |
| **Morador que não carregou no mês** | **Fatura zero**, sem mensalidade mínima nem taxa de disponibilidade. O sistema não distingue quem não tem EV de quem viajou: cobra apenas onde houve uso. |
| **Dois veículos da mesma unidade** | Cada morador é uma entidade individual, com cadastro, histórico e fatura próprios. Geram-se **duas faturas separadas**, atribuídas pelas reservas de cada um (consolidação em um pagador é preferência de pagamento, não de rateio). |
| **No-show de reserva** | Cartão não encostado em 20 min → reserva cancelada automaticamente e ponto liberado. Três no-shows em 30 dias → bloqueio temporário do direito de reservar (consequência comportamental, não financeira). |
### 6.5 Identificação e limite de cartões
O **cartão físico RFID** é um recurso compartilhado do condomínio (até 10 por carregador, custodiados pelo síndico). A **identidade do morador** vive no aplicativo, vinculada à reserva. O cruzamento entre reserva ativa e início de sessão (timestamp + Card ID) atribui a sessão à pessoa correta, escalando para qualquer número de moradores. Como rede de segurança, qualquer sessão sem reserva correspondente é atribuída ao dono cadastrado do cartão, evitando sessões órfãs.
---
## 7. O papel da IA na solução
A IA é adotada como **camada funcional estrutural, não decorativa**: cada técnica precisa atacar uma dor concreta que, sem ela, ficaria mal resolvida. Sob esse critério, **duas abordagens** se justificam, ambas operando sobre os dados que a SEMS e o gateway já entregam — sem instrumentação adicional. O detalhamento completo está no documento de apoio **`Papel_da_IA.docx`**.
### 7.1 Abordagem 1 — Previsão para recomendação de janela ótima
**Problema:** o incentivo da tarifação horária é passivo — o morador não consegue antecipar a curva solar, o estado da bateria ou o impacto financeiro de carregar agora versus de madrugada, e tende a carregar no pico, anulando o ganho.
**Técnica:** regressão sobre séries temporais (regressão linear com features sazonais e gradient boosting / XGBoost) para prever geração solar e consumo do prédio nas próximas 24–48 h, alimentando uma função-objetivo que minimiza o custo previsto sob a restrição de capacidade elétrica.
**Dados:** histórico de sessões, geração fotovoltaica, SOC da bateria, tarifas por faixa e previsão meteorológica (INMET).
**Impacto:** converte o incentivo abstrato em economia visível ("carregar às 2h30 custa R$ X contra R$ Y agora") e achata a curva de demanda no pico para o condomínio.
### 7.2 Abordagem 2 — Detecção de anomalias de medição
**Problema:** medições internamente coerentes para o equipamento, mas contextualmente impossíveis (ex.: 50 kWh em um carregador de 7 kW em 90 min), passam pelos testes internos do HCA G2 e entrariam silenciosamente na fatura.
**Técnica:** modelos não supervisionados de detecção de outliers (**Isolation Forest** e **Local Outlier Factor**, via PyOD e scikit-learn), que aprendem o normal a partir do histórico da planta sem exigir rótulos de fraude prévios.
**Dados:** kWh, duração, potência média/máxima, razão kWh/duração, faixa horária, Card ID e curva de potência.
**Impacto:** sessão anômala é bloqueada antes de entrar na fatura e fica suspensa para revisão humana, protegendo a integridade da cobrança e construindo a **trilha auditável exigida pela LGPD (art. 20)**.
### 7.3 Por que essas duas, e não outras
Ambas (a) operam sobre dados já disponíveis, (b) são centrais ao produto — sem a previsão, a tarifação horária perde efetividade; sem a detecção, a memória de cálculo deixa de ser confiável — e (c) são explicáveis, compatíveis com a LGPD. Clustering de perfis e chatbot por NLP foram considerados e descartados por ganho marginal ou risco de cair no critério de "decorativo".
---
## 8. Plano para a Sprint 02




---
## 9. Fontes consultadas
Todas as fontes abaixo foram efetivamente consultadas pela equipe. As referências completas de cada frente estão nos documentos `EV_ChargeOps_Frente1.docx`, `EV_ChargeOps_Frente2.docx` e `Frente_3_Arquitetura_e_IA.docx`.
**Frota, mercado e infraestrutura**
- ABVE — Associação Brasileira do Veículo Elétrico. Dados de frota e expansão da rede de recarga (2025–2026). https://abve.org.br
- McKinsey & Company. *Recarregar para crescer: o futuro da infraestrutura de recarga de veículos elétricos no Brasil* (2025). https://www.mckinsey.com/br/our-insights/all-insights/recarregar-para-crescer
- NeoCharge / Senatran. Crescimento e composição da frota de eletrificados. https://www.neocharge.com.br
- Tupi Mobilidade; Elétricos Brasil. Base de eletropostos e tempos de recarga.
**Soluções de mercado (benchmark)**
- Zaptec. *Zaptec Pro — Load and phase balancing for shared charging*. https://www.zaptec.com
- Wallbox. *Pulsar Plus / myWallbox*. https://wallbox.com
- ChargePoint. *Per-kWh billing and multifamily solutions*. https://www.chargepoint.com
- EMEL / LEVE. Operadora de pontos de carregamento de Lisboa (rede MOBI.E).
**Regulação setorial e energia**
- ANEEL. Resolução Normativa nº 1.000/2021. https://www2.aneel.gov.br/cedoc/ren20211000.html
- ANEEL. Resolução Normativa nº 1.059/2023. https://www2.aneel.gov.br/cedoc/ren20231059.html
- BRASIL. Lei nº 14.300/2022 (Marco Legal da Geração Distribuída).
- ANEEL. Tarifa Branca. https://www.gov.br/aneel/pt-br/assuntos/tarifas/tarifa-branca/introducao
**Tributação**
- BRASIL. Lei Complementar nº 116/2003 (item 14.01, ISS); Súmula 391 do STJ.
- BRASIL. Emenda Constitucional nº 132/2023; Lei Complementar nº 214/2025 (art. 28); Decreto nº 12.955/2026 (art. 15). https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2026/decreto/d12955.htm
- Sefaz-SP. Respostas a Consulta RC 30579/2024 e RC 31007/2024.
**Condominial e civil**
- BRASIL. Lei nº 10.406/2002 (Código Civil — condomínio edilício, arts. 1.331 e ss., 1.340, 1.342).
- SÃO PAULO (Estado). Lei nº 18.403/2026; SÃO PAULO (Município). Lei nº 17.336/2020.
- Câmara dos Deputados. PL 158/2025 e PL 6803/2025. https://www.camara.leg.br
**Normas técnicas**
- ABNT. NBR 5410, NBR 5419, NBR 17019, NBR IEC 61851 (partes 1–24), NBR IEC 62196; NR-10.
- Corpo de Bombeiros PMESP. Instrução Técnica IT-41 / Portaria 003/970/2026 (SAVE).
**Proteção de dados**
- BRASIL. Lei nº 13.709/2018 (LGPD — arts. 6º, 7º e 20).
**Programa industrial**
- BRASIL. Lei nº 14.902/2024 (Programa Mover); Decreto nº 12.435/2025. https://www.planalto.gov.br/ccivil_03/_Ato2023-2026/2024/Lei/L14902.htm
**Equipamento e plataforma GoodWe**
- GoodWe. HCA G2 Series AC Charger — página oficial e datasheet. https://en.goodwe.com/hca-g2
- GoodWe Technologies. Manual do usuário — Carregador CA Série HCA (7–22 kW) G2, V1.5 (11 nov. 2025).
- GoodWe. SEMS Portal / SEMS+. https://semsplus.goodwe.com
- Inspeção direta do ambiente *LAB FIAP Eco Smart Home* (SEMS+), realizada em 17/06/2026.
**Contraponto internacional e padrões**
- União Europeia. Regulation (EU) 2023/1804 (AFIR). https://transport.ec.europa.eu
- Estados Unidos. Programa NEVI (National Electric Vehicle Infrastructure).
- Open Charge Alliance. OCPP (1.6 / 2.0.1); ISO 15118 (Plug & Charge).
**Dados primários**
- Pesquisa de campo. Questionário aplicado a moradores de um condomínio residencial (78 respostas), jun. 2026. Elaboração própria.
