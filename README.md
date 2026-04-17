# Credit Card Request Process (BPMN + DMN)

## Overview
Este repositório apresenta a modelagem de um processo de solicitação de cartão de crédito projetado para ambientes de **alta disponibilidade** e **microsserviços**. O projeto utiliza **BPMN 2.0** para orquestração e conceitos de **DMN** para o desacoplamento de regras de decisão complexas.

A arquitetura foi refinada para demonstrar padrões de **resiliência** e **comunicação assíncrona**, garantindo que o sistema seja tolerante a falhas e escalável.

---

## Architectural Highlights

### Asynchronous & Event-Driven Design
* **Loose Coupling:** O Frontend e o Backend comunicam-se via **Message Flow**. O processo utiliza **Message Catch Events** para aguardar notificações de status sem bloquear a execução.
* **Callback Pattern:** Implementação de gatilhos de mensagem para sinalizar a conclusão das etapas de elegibilidade e criação de conta.

### Resilience & Fault Tolerance
* **Error Boundary Events:** Tratamento especializado para falhas críticas de infraestrutura (ex: APIs de terceiros fora do ar), diferenciando erros técnicos de negativas de negócio (Gateways).
* **Timer Events (Timeouts):** Mecanismos de proteção para evitar instâncias "órfãs". Se um serviço não responde no tempo esperado, o fluxo desvia para caminhos de recuperação ou notificação.
* **Non-blocking Audit:** Uso de **Parallel Gateways** para garantir que o registro de logs e auditoria não atrase a experiência final do cliente.

### Externalized Decision Logic (DMN)
* **Separation of Concerns:** As regras de elegibilidade e concessão de benefícios são mantidas fora do diagrama de fluxo, documentadas em tabelas de decisão.
* **Hit Policy (FIRST):** Estratégia de decisão clara para garantir que a primeira regra correspondente defina o resultado, evitando ambiguidades.

---

## Project Structure
```text
├── diagrams/
│   └── credit-card-process.bpmn  # Fluxo principal com padrões de resiliência
├── dmn/
│   ├── eligibility_rules.xlsx    # Regras de elegibilidade (DMN)
│   └── benefits_rules.xlsx       # Regras de atribuição de benefícios (DMN)
└── README.md
