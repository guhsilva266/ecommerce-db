# 🛒 E-commerce DB

Modelo relacional de banco de dados para um sistema de comércio eletrônico, desenvolvido no **MySQL Workbench**.

---

## 📋 Sobre o Projeto

Este projeto modela as principais entidades e relacionamentos de uma plataforma de e-commerce, abrangendo o cadastro de clientes, gerenciamento de produtos, pedidos, pagamentos, entregas, fornecedores e estoque.

---

## 🗂️ Estrutura do Banco de Dados

### Entidades Principais

| Tabela | Descrição |
|--------|-----------|
| `Cliente` | Dados base dos clientes |
| `Cliente PF` | Informações de clientes pessoa física |
| `Cliente PJ` | Informações de clientes pessoa jurídica |
| `Pedido` | Pedidos realizados pelos clientes |
| `Pagamento` | Dados de pagamento dos pedidos |
| `Entrega` | Informações de entrega, status e rastreio |
| `Produto` | Produtos cadastrados no sistema |
| `Fornecedor` | Fornecedores dos produtos |
| `Estoque` | Controle de localização de estoque |
| `Terceiro - vendedor` | Vendedores terceiros vinculados à plataforma |

### Tabelas de Relacionamento

| Tabela | Relacionamento |
|--------|----------------|
| `Relação de produto/Pedido` | Pedido ↔ Produto |
| `Disponibilizando um produto` | Fornecedor ↔ Produto |
| `Produtos por Vendedor (Terceiros)` | Terceiro vendedor ↔ Produto |
| `Produto_has_Estoque` | Produto ↔ Estoque |

---

## 🔗 Principais Relacionamentos

- Um **Cliente** pode ser cadastrado como **Pessoa Física (PF)** ou **Pessoa Jurídica (PJ)**
- Um **Cliente** pode realizar um ou mais **Pedidos**
- Um **Pedido** pode possuir informações de **Pagamento**
- Um **Pedido** possui dados de **Entrega**, incluindo status e código de rastreio
- Um **Pedido** pode conter um ou mais **Produtos**
- Um **Produto** pode estar vinculado a **Fornecedores**
- Um **Produto** pode ser disponibilizado por **Vendedores Terceiros**
- Um **Produto** pode estar associado ao **Estoque**

---

## 🛠️ Tecnologias

- [MySQL Workbench](https://www.mysql.com/products/workbench/) — modelagem EER
- MySQL — SGBD alvo

---

## 📁 Arquivos

```bash
📦 ecommerce-db
 ┣ 📄 README.md
 ┗ 🖼️ diagrama-ecommerce.png
```

---

## 🚀 Como Usar

1. Instale o [MySQL Workbench](https://dev.mysql.com/downloads/workbench/)
2. Abra o modelo do projeto no Workbench
3. Analise as entidades e relacionamentos do sistema
4. Caso deseje, gere o script SQL a partir do modelo usando `Database > Forward Engineer`

---

## 👤 Autor

**Gustavo Sampaio**  
Tecnologia em Gestão da Tecnologia da Informação (GTI)

---

## 📄 Licença

Este projeto está disponível para fins educacionais e de portfólio.
