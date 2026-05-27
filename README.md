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
<img width="420" height="545" alt="image" src="https://github.com/user-attachments/assets/a44fe893-fdc6-4d03-9a0a-6aa4a0c8626e" />

## Diagrama de Sequência — Fazer login
<img width="508" height="410" alt="image" src="https://github.com/user-attachments/assets/197cdeb3-6ce3-4094-a6a1-dbe54692375f" />

## Diagrama de Sequência — Agendar serviço
<img width="592" height="457" alt="image" src="https://github.com/user-attachments/assets/651f6cd4-ba38-4839-8af1-517462debd85" />

## Diagrama de Sequência — Cancelar agendamento
<img width="524" height="444" alt="image" src="https://github.com/user-attachments/assets/bb8f1306-b3d5-488f-a242-3f3de278acc8" />

## Diagrama de Classes
<img width="429" height="494" alt="image" src="https://github.com/user-attachments/assets/4704e1c8-c158-4dc7-9b0f-6748ed3779dd" />

## Diagrama de Componentes
<img width="704" height="467" alt="image" src="https://github.com/user-attachments/assets/d71463db-9714-42a0-b9b8-0b39ddef9285" />

## Diagrama de Implantação
<img width="434" height="397" alt="image" src="https://github.com/user-attachments/assets/5599c3f8-4a4b-4d06-961b-aaebb33dc987" />

## Diagrama de Estados — Agendamento
<img width="542" height="656" alt="image" src="https://github.com/user-attachments/assets/3694b6d6-93be-4e73-aad9-9d5580c8ac99" />

## Diagrama de Comunicação
<img width="592" height="204" alt="image" src="https://github.com/user-attachments/assets/c62a54f2-1521-4217-85d1-cb3f792eb3fa" />

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
