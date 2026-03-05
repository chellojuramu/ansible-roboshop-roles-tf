# RoboShop Ansible Roles: Role-Based Enterprise Architecture 🤖

This repository contains the professional-grade **Ansible Roles** configuration for the **RoboShop** microservices stack. The project has been refactored from standalone playbooks into a modular, industry-standard directory structure to ensure high reusability and maintainability.

## 🏗️ Repository Structure

Following the **DRY (Don't Repeat Yourself)** principle, the automation is organized into specialized components:

* **`roles/`**: Contains independent, reusable automation logic for each service (MongoDB, Redis, MySQL, RabbitMQ, Catalogue, etc.).
* **`group_vars/`**: Centralized location for global variables, separating configuration from automation logic.
* **`roboshop.yaml`**: The master orchestration playbook that calls the necessary roles in the correct dependency order.
* **`ansible.cfg`**: Project-level configuration to manage role paths and default settings.
* **`inventory.ini`**: Environment-specific host definitions.

## 🎯 Modular Automation Strategy

This project implements advanced Ansible features to manage a complex 3-tier application:

* **Common Role Integration**: Universal system tasks like `app-setup`, `nodejs-setup`, `java-setup`, and `python-setup` are extracted into a `common` role to be shared across all microservices.
* **Role Dependencies**: Specific services leverage the `meta` directory to automatically trigger prerequisite roles (e.g., ensuring `common` is ready before `shipping` starts).
* **Dynamic Templating**: Uses Jinja2 templates (`.j2`) to inject environment variables, database endpoints, and port configurations at runtime.
* **Reverse Proxy Logic**: Implements Nginx templates to route frontend traffic to backend microservices dynamically based on URI paths.
* **Idempotent Database Loading**: Uses conditional checks and `register` variables to ensure schemas (MongoDB/MySQL) are only loaded if they are missing.



## 🛠️ Technology Stack

* **Orchestration**: Ansible
* **Infrastructure**: AWS (EC2, Route53, Security Groups)
* **OS Standards**: RHEL 9 / CentOS (Enterprise Standard)
* **Languages**: NodeJS, Java (Maven), Python (uWSGI)
* **Web Server**: Nginx (Reverse Proxy)
* **Datastores**: MySQL, MongoDB, Redis, RabbitMQ

## 📂 Role Map

| Role | Responsibility | Tech Stack |
| :--- | :--- | :--- |
| **`common`** | Shared application and system-level setup logic (NodeJS, Java, Python preparers). | Shell / Ansible |
| **`mongodb`** | NoSQL database configuration and server installation. | NoSQL |
| **`mysql`** | Relational database setup with idempotent security checks. | SQL |
| **`catalogue`** | NodeJS microservice with idempotent MongoDB schema loading. | NodeJS |
| **`shipping`** | Java-based service built with Maven; handles MySQL schema migration. | Java / Maven |
| **`payment`** | Python-based microservice using uWSGI; connects to RabbitMQ. | Python |
| **`frontend`** | Nginx web server deploying static UI and managing reverse proxy rules. | Nginx |
| **`redis`** | In-memory cache configuration for session management. | In-Memory |
| **`rabbitmq`** | Message broker setup for asynchronous processing. | Erlang/AMQP |



## 🚀 Execution & Usage

### 1. Provision Infrastructure
Deploy the AWS resources using the automation script:

### To deploy all services using the centralized orchestration and ### Deploy Specific Service
To target a single microservice (e.g., Payment) utilizing the variable-driven logic :

```
ansible-playbook -e component=all roboshop.yaml 


ansible-playbook -e component=payment roboshop.yaml
