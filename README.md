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
<img width="684" height="536" alt="image" src="https://github.com/user-attachments/assets/f093aa16-d897-42cf-aadf-2964dc2d690f" />


## Diagrama de Sequência — Fazer login

<img width="1144" height="549" alt="image" src="https://github.com/user-attachments/assets/f4115cb9-0f20-49ef-ac2f-60f928825cce" />


## Diagrama de Sequência — Agendar serviço

<img width="1532" height="777" alt="image" src="https://github.com/user-attachments/assets/4a53cf61-b030-4304-85bf-4b0bdf9bfa37" />



## Diagrama de Sequência — Cancelar agendamento

<img width="1537" height="548" alt="image" src="https://github.com/user-attachments/assets/b25c1ae3-c8d8-4626-be0a-076b951da32c" />



## Diagrama de Classes

<img width="1534" height="786" alt="image" src="https://github.com/user-attachments/assets/e3560429-9738-4a84-9cc7-83042bfacef1" />



## Diagrama de Componentes

<img width="1250" height="875" alt="image" src="https://github.com/user-attachments/assets/5132ede5-63d8-4663-9d5e-019351ec1b73" />



## Diagrama de Implantação

<img width="860" height="709" alt="image" src="https://github.com/user-attachments/assets/6ad3214e-66a9-4c80-aee3-7f6e1e44e3fa" />


## Diagrama de Estados — Agendamento
<img width="916" height="656" alt="image" src="https://github.com/user-attachments/assets/68cfa431-07b6-498d-ad9d-d7c59d972aa8" />


## Diagrama de Comunicação

<img width="1046" height="282" alt="image" src="https://github.com/user-attachments/assets/0028dabe-65ea-4a9c-a5fc-34b699e8e092" />



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
