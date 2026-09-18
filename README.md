# MONITORAMENTO DE PRODUTIVIDADE EM TEMPO REAL: DASHBOARD PARA EFICIÊNCIA DE MÁQUINAS INDUSTRIAIS BASEADO EM COMPUTAÇÃO EM NUVEM

**Luis Gustavo de Lima Melo¹; Deivison Takatu²; Glauco²**

¹Discente do Curso de Tecnologia em Mecatrônica Industrial – Faculdade de Tecnologia SENAI Sorocaba "Santa Rosália"  
²Docente / Orientador – Faculdade de Tecnologia SENAI Sorocaba "Santa Rosália"  
*E-mails:* luis.melo@aluno.senai.br; deivison.takatu@sp.senai.br; glauco@sp.senai.br  

---

> **Resumo:** A Indústria 4.0 preconiza a digitalização e a rastreabilidade contínua dos processos fabris. Este trabalho propõe uma arquitetura serverless em nuvem para monitoramento de produtividade em tempo real, visando o cálculo automatizado do indicador OEE (Overall Equipment Effectiveness). A solução integra sensores eletromecânicos conectados via protocolo MQTT à plataforma AWS IoT Core, com processamento via AWS Lambda e persistência em banco NoSQL DynamoDB. O protótipo preliminar demonstrou viabilidade técnica com tempo de propagação inferior a 400 ms, viabilizando a gestão visual para pequenas e médias indústrias.  
> **Palavras-chave:** Indústria 4.0; OEE; IoT Industrial; Computação em Nuvem; Serverless.

---

### 1. INTRODUÇÃO E CONTEXTUALIZAÇÃO
O advento da Quarta Revolução Industrial transformou o chão de fábrica ao estabelecer a integração contínua entre componentes mecânicos e recursos computacionais (SCHWAB, 2016). Na manufatura contemporânea, a tomada de decisão em tempo hábil demanda visibilidade imediata das variáveis de desempenho dos ativos produtivos. Não obstante a disponibilidade de tecnologias avançadas, muitas pequenas e médias empresas ainda realizam a coleta de métricas fabris por meio de registros manuais em pranchetas ou planilhas descentralizadas, cenário que resulta em atrasos na detecção de paradas operacionais e elevado risco de imprecisão de registros (ALMEIDA et al., 2021).

### 2. PROBLEMA DE PESQUISA E OBJETIVOS
O problema investigado reside na elevada latência e carência de visibilidade em tempo real acerca do indicador de Eficiência Global do Equipamento (OEE — *Overall Equipment Effectiveness*). As soluções comerciais proprietárias (SCADA) impõem elevadas barreiras orçamentárias devido à necessidade de aquisição de servidores dedicados locais e licenciamento oneroso (LUCAS, 2019). O objetivo geral deste projeto de Iniciação Científica consiste em modelar, estruturar e avaliar uma arquitetura serverless de baixo custo em nuvem (AWS) capaz de capturar pulsos de máquinas industriais, computar o OEE em tempo real e fornecer um painel analítico interativo para apoio à gestão.

### 3. FUNDAMENTAÇÃO TEÓRICA
A Internet das Coisas Industrial (IIoT) viabiliza a extração direta de sinais em nível de campo por meio de microcontroladores e nós sensores (ASHTON, 2009). Ao desacoplar o armazenamento local por meio de computação em nuvem, minimiza-se a necessidade de gerenciar servidores físicos e viabiliza-se o escalonamento horizontal sob demanda (WORTMANN; FLÜCHTER, 2015). A adoção de paradigmas serverless reduz custos operacionais, garantindo que o processamento seja tarifado exclusivamente durante a execução de regras lógicas atreladas aos pulsos de produção fabril (SOUZA, 2022).

### 4. METODOLOGIA PROPOSTA
A pesquisa adota abordagem quantitativa e aplicada, estruturada nas seguintes etapas de desenvolvimento:
* **Camada de Borda (Edge):** Coleta de pulsos digitais de sensores industriais (contadores de ciclos e estado de máquina) conectados a um nó microcontrolado, estruturando payloads JSON e transmitindo via protocolo MQTT com criptografia TLS 1.3.
* **Ingestão e Processamento (AWS Cloud):** Recepção dos pacotes pelo broker AWS IoT Core, que dispara regras orientadas a eventos para funções serverless AWS Lambda responsáveis pelo cômputo dos pilares do OEE (Disponibilidade, Performance e Qualidade).
* **Armazenamento e Exibição:** Gravação em tabela NoSQL no Amazon DynamoDB para consultas rápidas de séries temporais e disponibilização dos dados através de uma API Gateway para renderização no dashboard web (React/Streamlit), acrescido de alertas via Amazon SNS.

```mermaid
graph LR
    A[Sensores de Campo] -->|MQTT / TLS| B(AWS IoT Core)
    B -->|Event Trigger| C{AWS Lambda}
    C --> D[(Amazon DynamoDB)]
    C --> E[Amazon SNS - Alertas]
    D --> F[Dashboard Web React]
    F --> G[Gestor de Produção]
