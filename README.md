# 📄 Relatório de Requisitos: PetRescue API

**Visão Geral do Sistema:**
O PetRescue é uma API RESTful projetada para conectar abrigos de animais a possíveis adotantes, além de gerenciar o histórico de saúde dos animais enquanto estão no abrigo através de um módulo veterinário.

## 👥 Perfis de Usuário (Atores)
* **Administrador (Abrigo):** Gerencia o cadastro de animais, aprova/rejeita pedidos de adoção e gerencia usuários.
* **Veterinário:** Registra consultas, vacinas e o histórico médico dos animais do abrigo.
* **Adotante:** Visualiza os animais disponíveis e envia formulários de intenção de adoção.

---

## ⚙️ Requisitos Funcionais (RF)

| ID | Nome do Requisito | Descrição | Ator |
| :--- | :--- | :--- | :--- |
| **RF01** | Gestão de Animais | O sistema deve permitir o CRUD de perfis de animais, incluindo fotos, espécie, raça, idade e status. | Administrador |
| **RF02** | Catálogo Público | O sistema deve permitir a listagem filtrada de animais com status "Disponível". | Todos |
| **RF03** | Gestão de Usuários | O sistema deve permitir o cadastro de adotantes e a criação interna de perfis de veterinários e administradores. | Todos / Admin |
| **RF04** | Solicitação de Adoção | O sistema deve permitir que um Adotante envie um formulário de intenção de adoção. | Adotante |
| **RF05** | Avaliação de Adoção | O sistema deve permitir que o Administrador aprove ou rejeite uma solicitação, alterando o status do animal automaticamente. | Administrador |
| **RF06** | Prontuário Médico | O sistema deve permitir que o Veterinário adicione registros de consultas, diagnósticos e vacinas aplicadas a um animal. | Veterinário |

---

## 🔒 Requisitos Não Funcionais (RNF)

* **RNF01 - Arquitetura:** API RESTful utilizando o padrão arquitetural MVC (Controller, Service, Repository).
* **RNF02 - Tecnologia Base:** Java 17+ e Spring Boot 3.x.
* **RNF03 - Segurança:** Rotas protegidas via **Spring Security** com **JWT (JSON Web Token)** e senhas com hash BCrypt.
* **RNF04 - Persistência:** Banco de dados relacional (PostgreSQL/MySQL) com **Spring Data JPA** e Hibernate.
* **RNF05 - Validação:** DTOs validados via **Spring Boot Validation** (`@NotBlank`, `@Email`, etc).
* **RNF06 - Documentação:** Documentação via **Swagger/OpenAPI** (`springdoc-openapi`).

---

## 🗂️ Modelo de Dados (Entidades Principais)

1. **User:** `id`, `name`, `email`, `password`, `role` (Enum: ADMIN, VET, ADOPTER).
2. **Pet:** `id`, `name`, `species`, `breed`, `age`, `description`, `status` (Enum: AVAILABLE, PENDING, ADOPTED).
3. **AdoptionApplication:** `id`, `adopterId` (User), `petId` (Pet), `applicationDate`, `status` (Enum: SUBMITTED, APPROVED, REJECTED), `motivationText`.
4. **MedicalRecord:** `id`, `petId` (Pet), `vetId` (User), `date`, `description`, `treatmentType` (Enum: VACCINE, CHECKUP, SURGERY).

---

## 🚀 Dicas de Implementação no Spring Boot

* **Separação de Responsabilidades:** Nunca exponha Entidades (Models) diretamente nos Controllers. Crie classes **DTO**.
* **Tratamento de Exceções:** Crie um `@ControllerAdvice` global para capturar exceções e retornar JSON padronizado.
* **Paginação:** Utilize a interface `Pageable` do Spring Data JPA nas listagens do Catálogo Público.

