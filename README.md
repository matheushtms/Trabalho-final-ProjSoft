# BeautyBook

## 1. Introdução

O BeautyBook é um sistema pensado para ajudar um salão de beleza a organizar melhor os agendamentos, os clientes e os serviços oferecidos. A ideia é deixar o atendimento mais prático, evitando confusão com horários, encaixes e cancelamentos.

Nesse sistema, o cliente consegue marcar seu horário de forma mais fácil, enquanto o salão consegue visualizar os atendimentos do dia, controlar os serviços e acompanhar os pagamentos.

O projeto foi montado apenas na parte de análise, diagramação e arquitetura, sem desenvolvimento de código. Para isso, foram usados os conceitos vistos na disciplina e a modelagem foi feita com PlantUML.

---

# 2. Modelos de Usuário e Requisitos

## 2.1 Descrição de Atores

### Cliente
Pessoa que acessa o sistema para:
- Fazer login
- Agendar serviços
- Cancelar horários
- Acompanhar seus atendimentos

### Funcionário
Responsável por:
- Atender os clientes
- Confirmar agendamentos
- Atualizar informações do sistema

### Administrador
Cuida da parte geral do sistema, como:
- Cadastro de usuários
- Controle de serviços
- Relatórios
- Organização do salão

---

## 2.2 Casos de Uso

Os principais casos de uso do sistema são:

- Fazer login
- Cadastrar cliente
- Agendar serviço
- Cancelar agendamento
- Confirmar atendimento
- Registrar pagamento
- Gerar relatório

---

## 2.3 Sequência do Sistema

Foram criados diagramas de sequência para mostrar como o sistema funciona em situações do dia a dia, como login, agendamento e cancelamento. Esses diagramas ajudam a entender a ordem das interações entre o usuário e o sistema.


## Diagrama de Casos de Uso

📄 [Código-Fonte PlantUML](./diagrams/use_case.puml)

```mermaid
graph LR
    classDef actor fill:#3F51B5,stroke:#303F9F,stroke-width:2px,color:#fff;
    classDef usecase fill:#FAFAFA,stroke:#3F51B5,stroke-width:1px,color:#303F9F;
    
    Cliente:::actor
    Funcionario["Funcionário"]:::actor
    Admin["Administrador"]:::actor
    
    subgraph BB["BeautyBook - Sistema de Salão de Beleza"]
        Login["Fazer Login"]:::usecase
        CadCli["Cadastrar Cliente"]:::usecase
        Agendar["Agendar Serviço"]:::usecase
        Cancelar["Cancelar Agendamento"]:::usecase
        Confirmar["Confirmar Atendimento"]:::usecase
        Pagamento["Registrar Pagamento"]:::usecase
        Relatorio["Gerar Relatório"]:::usecase
        Validar["Validar Credenciais"]:::usecase
        Notificar["Notificar por E-mail/SMS"]:::usecase
    end
    
    Cliente --> Login
    Cliente --> CadCli
    Cliente --> Agendar
    Cliente --> Cancelar
    
    Funcionario --> Login
    Funcionario --> Agendar
    Funcionario --> Cancelar
    Funcionario --> Confirmar
    Funcionario --> Pagamento
    
    Admin --> Login
    Admin --> CadCli
    Admin --> Confirmar
    Admin --> Pagamento
    Admin --> Relatorio
    
    Login -.->|&lt;&lt;include&gt;&gt;| Validar
    Agendar -.->|&lt;&lt;extend&gt;&gt;| Notificar
    Cancelar -.->|&lt;&lt;extend&gt;&gt;| Notificar
```

## Diagrama de Sequência — Fazer login

📄 [Código-Fonte PlantUML](./diagrams/seq_login.puml)

```mermaid
sequenceDiagram
    autonumber
    actor Usuario as Usuário
    participant Front as Interface de Login (Frontend)
    participant Auth as AuthController (Backend)
    participant DB as Banco de Dados
    
    Usuario->>Front: Digita email e senha e clica em "Entrar"
    activate Front
    Front->>Auth: POST /api/auth/login (email, senha)
    activate Auth
    Auth->>DB: Buscar usuário por email
    activate DB
    DB-->>Auth: Retorna dados (hash da senha)
    deactivate DB
    
    Note over Auth: Validar credenciais (verificar hash)
    
    alt Credenciais Válidas
        Note over Auth: Gerar Token JWT
        Auth-->>Front: 200 OK (token, dadosUsuario)
        Front->>Usuario: Redireciona para o Dashboard (Home)
    else Credenciais Inválidas
        Auth-->>Front: 401 Unauthorized (Erro)
        deactivate Auth
        Front->>Usuario: Exibe mensagem de erro
        deactivate Front
    end
```

## Diagrama de Sequência — Agendar serviço

📄 [Código-Fonte PlantUML](./diagrams/seq_agendar.puml)

```mermaid
sequenceDiagram
    autonumber
    actor Cliente as Cliente
    participant Front as Interface de Agendamento (Frontend)
    participant Backend as AgendamentoController (Backend)
    participant DB as Banco de Dados
    participant Notif as NotificationService
    
    Cliente->>Front: Acessa a página de agendamento
    activate Front
    Front->>Backend: GET /api/servicos e GET /api/funcionarios
    activate Backend
    Backend->>DB: Consultar serviços e funcionários ativos
    activate DB
    DB-->>Backend: Lista de serviços e funcionários
    deactivate DB
    Backend-->>Front: Retorna serviços e funcionários (200 OK)
    deactivate Backend
    
    Cliente->>Front: Seleciona Serviço, Funcionário e Data/Hora
    Front->>Backend: POST /api/agendamentos/validar (dados)
    activate Backend
    Backend->>DB: Verificar conflitos de horário
    activate DB
    DB-->>Backend: 0 conflitos
    deactivate DB
    
    alt Horário Disponível
        Backend-->>Front: Horário Disponível (200 OK)
        Cliente->>Front: Confirma o agendamento
        Front->>Backend: POST /api/agendamentos (criar)
        Backend->>DB: INSERT INTO agendamentos (status = 'PENDENTE')
        activate DB
        DB-->>Backend: Retorna ID
        deactivate DB
        
        Backend->>Notif: Enviar email/SMS de confirmação
        activate Notif
        Notif-->>Backend: Confirma envio
        deactivate Notif
        
        Backend-->>Front: 201 Created (Sucesso)
        Front->>Cliente: Exibe mensagem de sucesso
    else Horário Indisponível
        Backend-->>Front: Horário Indisponível (409 Conflict)
        deactivate Backend
        Front->>Cliente: Exibe mensagem de erro: "Horário ocupado"
        deactivate Front
    end
```

## Diagrama de Sequência — Cancelar agendamento

📄 [Código-Fonte PlantUML](./diagrams/seq_cancelar.puml)

```mermaid
sequenceDiagram
    autonumber
    actor Actor as Cliente / Funcionário
    participant Front as Interface de Agendamentos (Frontend)
    participant Backend as AgendamentoController (Backend)
    participant DB as Banco de Dados
    participant Notif as NotificationService
    
    Actor->>Front: Clica em "Cancelar Agendamento"
    activate Front
    Front->>Actor: Solicita confirmação de cancelamento
    Actor->>Front: Confirma cancelamento
    Front->>Backend: POST /api/agendamentos/{id}/cancelar
    activate Backend
    
    Backend->>DB: Buscar detalhes do agendamento por ID
    activate DB
    DB-->>Backend: Retorna dados (dataHora, status, ids)
    deactivate DB
    
    Note over Backend: Validar regra de antecedência (> 2 horas)
    
    alt Antecedência Válida (>= 2 horas)
        Backend->>DB: UPDATE agendamentos SET status = 'CANCELADO'
        activate DB
        DB-->>Backend: Confirmação de atualização
        deactivate DB
        Backend->>Notif: Enviar notificação de cancelamento
        activate Notif
        Notif-->>Backend: Confirmação de envio
        deactivate Notif
        Backend-->>Front: 200 OK (Cancelamento confirmado)
        Front->>Actor: Exibe sucesso e atualiza a lista
    else Antecedência Inválida (< 2 horas)
        Backend-->>Front: 400 Bad Request (Prazo não cumprido)
        deactivate Backend
        Front->>Actor: Exibe erro: "Cancelamento indisponível"
        deactivate Front
    end
```

## Diagrama de Classes

📄 [Código-Fonte PlantUML](./diagrams/classes.puml)

```mermaid
classDiagram
    class Usuario {
        <<abstract>>
        -Int id
        -String nome
        -String email
        -String senha
        -String telefone
        +login(email, senha) Boolean
    }
    class Cliente {
        -Date dataCadastro
        +cadastrar() Boolean
        +solicitarAgendamento(servico, dataHora) Agendamento
        +solicitarCancelamento(agendamento) Boolean
    }
    class Funcionario {
        -String especialidade
        -String horarioTrabalho
        +confirmarAgendamento(agendamento) Boolean
        +registrarAtendimento(agendamento) Boolean
    }
    class Administrador {
        -String nivelAcesso
        +cadastrarFuncionario(func) Boolean
        +cadastrarServico(servico) Boolean
        +gerarRelatorio(dataInicio, dataFim) Relatorio
    }
    class Servico {
        -Int id
        -String nome
        -String descricao
        -Double preco
        -Int duracaoMinutos
        +cadastrar() Boolean
        +atualizarPreco(novoPreco) Boolean
    }
    class Agendamento {
        -Int id
        -DateTime dataHora
        -StatusAgendamento status
        +alterarStatus(novoStatus)
        +associarPagamento(pagamento)
    }
    class Pagamento {
        -Int id
        -Double valor
        -DateTime dataHora
        -StatusPagamento status
        -MetodoPagamento metodo
        +processarPagamento() Boolean
        +estornarPagamento() Boolean
    }
    class Relatorio {
        -Int id
        -Date dataGeracao
        -String tipo
        -String conteudo
        +exportarPDF() Byte[]
    }
    
    class StatusAgendamento {
        <<enumeration>>
        PENDENTE
        CONFIRMADO
        EM_ATENDIMENTO
        FINALIZADO
        CANCELADO
    }
    class StatusPagamento {
        <<enumeration>>
        PENDENTE
        PAGO
        ESTORNADO
        FALHADO
    }
    class MetodoPagamento {
        <<enumeration>>
        DINHEIRO
        CARTAO_CREDITO
        CARTAO_DEBITO
        PIX
    }
    
    Usuario <|-- Cliente
    Usuario <|-- Funcionario
    Usuario <|-- Administrador
    
    Cliente "1" -- "0..*" Agendamento : realiza
    Funcionario "1" -- "0..*" Agendamento : executa
    Servico "1" -- "0..*" Agendamento : compoe
    Agendamento "1" *-- "0..1" Pagamento : possui
    Administrador ..> Relatorio : gera
    
    Agendamento ..> StatusAgendamento
    Pagamento ..> StatusPagamento
    Pagamento ..> MetodoPagamento
```

## Diagrama de Componentes

📄 [Código-Fonte PlantUML](./diagrams/componentes.puml)

```mermaid
graph TD
    subgraph UI["Interface de Usuário (Frontend SPA)"]
        SPA["Interface Web (SPA)"]
    end
    
    subgraph Core["Backend API (BeautyBook Core)"]
        AuthCtrl["Controller de Autenticação"]
        AgendaCtrl["Controller de Agendamentos"]
        CliCtrl["Controller de Clientes"]
        PagCtrl["Controller de Pagamentos"]
        RelCtrl["Controller de Relatórios"]
        
        AgendaMod["Módulo de Agendamentos"]
        CliMod["Módulo de Clientes"]
        PagMod["Módulo de Pagamentos"]
        RelMod["Módulo de Relatórios"]
        NotifService["Serviço de Notificações"]
    end
    
    subgraph Ext["Serviços Externos"]
        ExtPay["Gateway de Pagamento (MercadoPago/Stripe)"]
        ExtNotif["Serviço de SMS/E-mail (Twilio/SendGrid)"]
    end
    
    subgraph DB["Banco de Dados (PostgreSQL)"]
        DBSchema[("Tabelas / Esquema Relacional")]
    end
    
    SPA -->|HTTP/REST| AuthCtrl
    SPA -->|HTTP/REST| AgendaCtrl
    SPA -->|HTTP/REST| CliCtrl
    SPA -->|HTTP/REST| PagCtrl
    SPA -->|HTTP/REST| RelCtrl
    
    AgendaCtrl --> AgendaMod
    CliCtrl --> CliMod
    PagCtrl --> PagMod
    RelCtrl --> RelMod
    
    AgendaMod --> NotifService
    AgendaMod --> CliMod
    PagMod --> AgendaMod
    
    PagMod -->|HTTPS API| ExtPay
    NotifService -->|HTTPS API| ExtNotif
    
    AuthCtrl --> DBSchema
    AgendaMod --> DBSchema
    CliMod --> DBSchema
    PagMod --> DBSchema
    RelMod --> DBSchema
    
    classDef layer fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef component fill:#FAFAFA,stroke:#3F51B5,stroke-width:1px,color:#212121;
    classDef database fill:#E0F2F1,stroke:#009688,stroke-width:1px,color:#212121;
    
    SPA:::component
    AuthCtrl:::component; AgendaCtrl:::component; CliCtrl:::component; PagCtrl:::component; RelCtrl:::component;
    AgendaMod:::component; CliMod:::component; PagMod:::component; RelMod:::component; NotifService:::component;
    ExtPay:::component; ExtNotif:::component;
    DBSchema:::database
```

## Diagrama de Implantação

📄 [Código-Fonte PlantUML](./diagrams/implantacao.puml)

```mermaid
graph TD
    subgraph UserDevice["Dispositivo do Usuário (PC / Mobile)"]
        subgraph Browser["Navegador Web"]
            SPA["SPA BeautyBook (HTML5/React)"]
        end
    end
    
    subgraph HostingFront["Hospedagem Frontend (Vercel / Netlify)"]
        Static["Arquivos Estáticos SPA"]
    end
    
    subgraph CloudApp["Servidor de Aplicação (Render / AWS EC2)"]
        subgraph Runtime["Runtime (Node.js / JVM)"]
            API["BeautyBook API (.js / .jar)"]
        end
    end
    
    subgraph CloudDB["Banco de Dados Gerenciado"]
        DBMS[("PostgreSQL Database")]
    end
    
    UserDevice -->|1: GET / (HTTPS)| HostingFront
    SPA -->|2: REST Request (HTTPS - Porta 443)| API
    API -->|3: JDBC/SQL (Porta 5432)| DBMS
    
    classDef node fill:#FAFAFA,stroke:#3F51B5,stroke-width:2px;
    classDef dbNode fill:#E0F2F1,stroke:#009688,stroke-width:2px;
    classDef art fill:#E8EAF6,stroke:#303F9F,stroke-width:1px;
    
    UserDevice:::node; HostingFront:::node; CloudApp:::node; CloudDB:::dbNode;
    SPA:::art; API:::art; DBMS:::dbNode
```

## Diagrama de Estados — Agendamento

📄 [Código-Fonte PlantUML](./diagrams/estados.puml)

```mermaid
stateDiagram-v2
    [*] --> Pendente : solicitarAgendamento()
    
    Pendente --> Confirmado : confirmarAgendamento()
    Pendente --> Cancelado : solicitarCancelamento()
    
    Confirmado --> EmAtendimento : registrarAtendimento()
    Confirmado --> Cancelado : solicitarCancelamento()\n[antecedência >= 2h]
    
    EmAtendimento --> Finalizado : registrarConclusao()
    
    Finalizado --> [*]
    Cancelado --> [*]
    
    note right of Pendente : entry: gerar registro pendente\nexit: notificar funcionário
    note right of Confirmado : entry: bloquear horário\nexit: enviar lembrete 2h antes
    note right of EmAtendimento : entry: registrar início do serviço\nexit: registrar fim do serviço
    note right of Finalizado : entry: gerar cobrança de pagamento
    note right of Cancelado : entry: liberar horário e notificar partes
```

## Diagrama de Comunicação

📄 [Código-Fonte PlantUML](./diagrams/comunicacao.puml)

```mermaid
graph TD
    Cliente["c : Cliente"]
    View["view : InterfaceAgendamento"]
    Ctrl["ctrl : AgendamentoController"]
    Servico["s : Servico"]
    Agendamento["a : Agendamento"]
    Notif["notif : NotificationService"]
    
    Cliente -->|1: solicitarAgendamento()| View
    View -->|2: criarAgendamento()| Ctrl
    Ctrl -->|3: obterDetalhesServico()| Servico
    Ctrl -->|4: <<create>> novo()| Agendamento
    Ctrl -->|5: enviarConfirmacao()| Notif
    Ctrl -->|6: exibirSucesso()| View
    View -->|7: mostrarConfirmacao()| Cliente
    
    classDef obj fill:#FAFAFA,stroke:#3F51B5,stroke-width:1px,color:#212121;
    Cliente:::obj; View:::obj; Ctrl:::obj; Servico:::obj; Agendamento:::obj; Notif:::obj;
```

---

# 3. Modelos de Projeto

## 3.1 Arquitetura

A arquitetura do BeautyBook foi pensada de forma simples e organizada, usando uma estrutura em camadas.

A parte visual fica no frontend, a lógica do sistema fica no backend, e os dados são guardados no banco de dados. Essa separação ajuda bastante na organização do projeto e facilita alterações futuras.

---

## 3.2 Componentes e Implantação

O sistema foi dividido em componentes principais:

- Interface web
- API
- Módulos de agendamento
- Clientes
- Pagamentos

Na implantação, o cliente acessa o sistema pelo navegador, que se comunica com o servidor de aplicação. Esse servidor, por sua vez, conversa com o banco de dados.

---

## 3.3 Diagrama de Classes

O diagrama de classes mostra os principais objetos do sistema e como eles se relacionam.

As classes principais são:
- Cliente
- Funcionário
- Serviço
- Agendamento
- Pagamento

---

## 3.4 Diagramas de Sequência

Os diagramas de sequência mostram o passo a passo de algumas ações importantes do sistema, como:
- Login
- Agendamento
- Cancelamento

---

## 3.5 Diagramas de Comunicação

Esse diagrama foi usado para mostrar como os objetos trocam mensagens durante a execução das funcionalidades.

---

## 3.6 Diagramas de Estados

O diagrama de estados mostra como um agendamento pode mudar ao longo do tempo, começando como criado, depois confirmado, em atendimento e finalizado, ou então cancelado.

---

# 4. Modelos de Dados

O banco de dados do BeautyBook foi pensado com tabelas para:
- Clientes
- Funcionários
- Serviços
- Agendamentos
- Pagamentos

A ideia é guardar tudo de forma organizada para que o sistema consiga consultar, atualizar e relacionar as informações sem dificuldade.
