# 🛒 E-commerce DB

Modelo relacional completo de banco de dados para um sistema de comércio eletrônico, desenvolvido no **MySQL Workbench**. Este projeto demonstra a aplicação de conceitos avançados de modelagem de dados, incluindo specialização de entidades, relacionamentos complexos e padrões de design de banco de dados.

**Status:** ✅ Versão 1.0 — Modelo consolidado e documentado

---

## 📋 Sumário

- [Visão Geral](#visão-geral)
- [Estrutura do Banco de Dados](#estrutura-do-banco-de-dados)
- [Diagrama EER](#diagrama-eer)
- [Entidades Principais](#entidades-principais)
- [Relacionamentos](#relacionamentos)
- [Como Usar](#como-usar)
- [Tecnologias](#tecnologias)
- [Exemplos de Queries](#exemplos-de-queries)
- [Melhorias Futuras](#melhorias-futuras)
- [Autor](#autor)
- [Licença](#licença)

---

## 🎯 Visão Geral

Este projeto modela uma plataforma de e-commerce moderna, abrangendo todas as operações essenciais: cadastro de clientes (com distinção PF/PJ), gerenciamento de produtos, processamento de pedidos, controle de pagamentos, rastreamento de entregas, gestão de fornecedores e controle de estoque.

**Objetivo:** Servir como referência para desenvolvimento de sistemas de comércio eletrônico, demonstrando boas práticas em modelagem relacional e design de dados.

---

## 🗂️ Estrutura do Banco de Dados

### 📊 Diagrama EER

![Diagrama E-commerce](diagrama-ecommerce.png)

*Diagrama da estrutura relacional do sistema de e-commerce*

---

## 📑 Entidades Principais

### Clientes

| Tabela | Descrição |
|--------|-----------|
| `Cliente` | Entidade base com dados comuns a PF e PJ |
| `Cliente_PF` | Especialização para pessoas físicas (CPF, RG, data de nascimento) |
| `Cliente_PJ` | Especialização para pessoas jurídicas (CNPJ, razão social, inscrição estadual) |

**Padrão:** Generalização/Especialização (atributos comuns em `Cliente`, atributos específicos nas tabelas filhas)

---

### Produtos e Fornecimento

| Tabela | Descrição |
|--------|-----------|
| `Produto` | Produtos cadastrados no catálogo |
| `Fornecedor` | Fornecedores que abastecem produtos |
| `Disponibilizando_Produto` | Relacionamento N:N entre Fornecedor e Produto |
| `Estoque` | Localizações de armazenamento e quantidades |
| `Produto_has_Estoque` | Relacionamento N:N entre Produto e Estoque |

---

### Pedidos e Transações

| Tabela | Descrição |
|--------|-----------|
| `Pedido` | Pedidos realizados pelos clientes |
| `Relacao_Produto_Pedido` | Itens dentro de um pedido (N:N) |
| `Pagamento` | Informações de pagamento associadas ao pedido |
| `Entrega` | Status e rastreamento de entregas |

---

### Vendedores Terceiros

| Tabela | Descrição |
|--------|-----------|
| `Terceiro_Vendedor` | Vendedores terceiros na plataforma |
| `Produtos_por_Vendedor` | Produtos disponibilizados por terceiros |

---

## 🔗 Relacionamentos

### Relacionamentos Principais

```
Cliente (1) ──→ (N) Pedido
           ├─→ Cliente_PF
           └─→ Cliente_PJ

Pedido (1) ──→ (N) Pagamento
       ├─→ (N) Entrega
       └─→ (N) Relacao_Produto_Pedido

Produto (1) ──→ (N) Relacao_Produto_Pedido
        ├─→ (N) Disponibilizando_Produto
        ├─→ (N) Produtos_por_Vendedor
        └─→ (N) Produto_has_Estoque

Fornecedor (1) ──→ (N) Disponibilizando_Produto

Terceiro_Vendedor (1) ──→ (N) Produtos_por_Vendedor

Estoque (1) ──→ (N) Produto_has_Estoque
```

### Cardinalidades

- **1:N** — Um cliente para múltiplos pedidos
- **N:N** — Um produto em múltiplos pedidos (com quantidade)
- **N:N** — Um produto fornecido por múltiplos fornecedores
- **Generalização** — Cliente com especialização em PF/PJ

---

## 🚀 Como Usar

### 📥 Pré-requisitos

- [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) (versão 8.0+)
- MySQL Server 5.7+ ou MariaDB 10.3+
- Git (opcional, para clonar o repositório)

### 📖 Passos

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/guhsilva266/ecommerce-db.git
   cd ecommerce-db
   ```

2. **Abra o MySQL Workbench** e carregue o arquivo do modelo (`.mwb`)

3. **Analise o diagrama EER:**
   - Veja as tabelas e seus relacionamentos
   - Estude as chaves primárias e estrangeiras
   - Observe os padrões de design aplicados

4. **Gere o script SQL** (opcional):
   - No Workbench, vá em `Database` → `Forward Engineer`
   - Selecione as opções desejadas
   - Exporte o script ou execute direto no seu servidor

5. **Execute o script no seu banco:**
   ```sql
   -- No MySQL CLI ou cliente SQL
   mysql -u root -p < ecommerce-db.sql
   ```

---

## 🛠️ Tecnologias

| Tecnologia | Descrição |
|-----------|-----------|
| **MySQL Workbench** | Ferramenta de modelagem EER e design de banco de dados |
| **MySQL** | SGBD relacional alvo |
| **Git & GitHub** | Controle de versão e hospedagem |

---

## 📊 Detalhes das Principais Tabelas

### Cliente
```sql
CREATE TABLE Cliente (
  idCliente INT PRIMARY KEY AUTO_INCREMENT,
  email VARCHAR(100) UNIQUE NOT NULL,
  telefone VARCHAR(15),
  endereco VARCHAR(200),
  dataCadastro TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  status ENUM('ATIVO', 'INATIVO') DEFAULT 'ATIVO'
);
```

### Cliente_PF (Pessoa Física)
```sql
CREATE TABLE Cliente_PF (
  idCliente_PF INT PRIMARY KEY AUTO_INCREMENT,
  idCliente INT UNIQUE NOT NULL,
  cpf VARCHAR(14) UNIQUE NOT NULL,
  rg VARCHAR(12),
  dataNascimento DATE,
  FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente)
);
```

### Cliente_PJ (Pessoa Jurídica)
```sql
CREATE TABLE Cliente_PJ (
  idCliente_PJ INT PRIMARY KEY AUTO_INCREMENT,
  idCliente INT UNIQUE NOT NULL,
  cnpj VARCHAR(18) UNIQUE NOT NULL,
  razaoSocial VARCHAR(150) NOT NULL,
  inscricaoEstadual VARCHAR(20),
  FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente)
);
```

### Produto
```sql
CREATE TABLE Produto (
  idProduto INT PRIMARY KEY AUTO_INCREMENT,
  nome VARCHAR(150) NOT NULL,
  descricao TEXT,
  preco DECIMAL(10, 2) NOT NULL,
  peso DECIMAL(8, 2),
  dimensoes VARCHAR(50),
  status ENUM('ATIVO', 'INATIVO') DEFAULT 'ATIVO'
);
```

### Pedido
```sql
CREATE TABLE Pedido (
  idPedido INT PRIMARY KEY AUTO_INCREMENT,
  idCliente INT NOT NULL,
  dataPedido TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  valorTotal DECIMAL(12, 2),
  statusPedido ENUM('PENDENTE', 'PROCESSANDO', 'ENVIADO', 'ENTREGUE', 'CANCELADO') DEFAULT 'PENDENTE',
  FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente)
);
```

### Pagamento
```sql
CREATE TABLE Pagamento (
  idPagamento INT PRIMARY KEY AUTO_INCREMENT,
  idPedido INT NOT NULL,
  metodoPagamento ENUM('CREDITO', 'DEBITO', 'BOLETO', 'PIX', 'DINHEIRO') NOT NULL,
  valor DECIMAL(12, 2) NOT NULL,
  dataPagamento TIMESTAMP,
  statusPagamento ENUM('PENDENTE', 'APROVADO', 'RECUSADO', 'REEMBOLSADO') DEFAULT 'PENDENTE',
  FOREIGN KEY (idPedido) REFERENCES Pedido(idPedido)
);
```

### Entrega
```sql
CREATE TABLE Entrega (
  idEntrega INT PRIMARY KEY AUTO_INCREMENT,
  idPedido INT NOT NULL UNIQUE,
  dataEnvio DATE,
  dataEntrega DATE,
  statusEntrega ENUM('PROCESSANDO', 'ENVIADO', 'ENTREGUE', 'DEVOLVIDO') DEFAULT 'PROCESSANDO',
  codigoRastreio VARCHAR(50) UNIQUE,
  transportadora VARCHAR(100),
  FOREIGN KEY (idPedido) REFERENCES Pedido(idPedido)
);
```

---

## 💡 Exemplos de Queries

### Listar todos os clientes e seus pedidos

```sql
SELECT 
  c.idCliente,
  c.email,
  COUNT(p.idPedido) AS total_pedidos,
  SUM(p.valorTotal) AS valor_total_gasto
FROM Cliente c
LEFT JOIN Pedido p ON c.idCliente = p.idCliente
GROUP BY c.idCliente
ORDER BY valor_total_gasto DESC;
```

### Produtos mais vendidos

```sql
SELECT 
  pr.idProduto,
  pr.nome,
  COUNT(rpp.idProduto) AS quantidade_vendida,
  SUM(rpp.quantidade) AS total_itens
FROM Produto pr
JOIN Relacao_Produto_Pedido rpp ON pr.idProduto = rpp.idProduto
GROUP BY pr.idProduto
ORDER BY quantidade_vendida DESC
LIMIT 10;
```

### Status de pedidos e entregas

```sql
SELECT 
  pd.idPedido,
  c.email,
  pd.dataPedido,
  pd.statusPedido,
  e.statusEntrega,
  e.codigoRastreio
FROM Pedido pd
JOIN Cliente c ON pd.idCliente = c.idCliente
LEFT JOIN Entrega e ON pd.idPedido = e.idPedido
WHERE pd.statusPedido = 'ENVIADO'
ORDER BY pd.dataPedido DESC;
```

### Fornecedores por produto

```sql
SELECT 
  pr.idProduto,
  pr.nome,
  f.nomeEmpresa,
  dp.precoFornecimento
FROM Produto pr
JOIN Disponibilizando_Produto dp ON pr.idProduto = dp.idProduto
JOIN Fornecedor f ON dp.idFornecedor = f.idFornecedor
ORDER BY pr.nome, f.nomeEmpresa;
```

---

## 🎨 Padrões e Conceitos Aplicados

### ✅ Boas Práticas Implementadas

- **Normalização:** Banco segue até 3ª forma normal (3NF)
- **Chaves Primárias:** Todas as tabelas possuem PK bem definida
- **Integridade Referencial:** Relacionamentos mantidos com FK
- **Generalização/Especialização:** Padrão de herança aplicado em Cliente
- **Tabelas de Relacionamento:** N:N corretamente modelados
- **Enumerações:** Uso de ENUM para status e categorias fixas
- **Índices Implícitos:** PKs e FKs indexadas automaticamente

---

## 🚀 Melhorias Futuras

Ideias para expansão do modelo:

- [ ] **Tabela `Avaliacao`** — Reviews e ratings de clientes
- [ ] **Tabela `Categoria`** — Categorização hierárquica de produtos
- [ ] **Tabela `Cupom`** — Cupons e códigos de desconto
- [ ] **Tabela `Historico_Preco`** — Rastreamento de variação de preços
- [ ] **Soft Delete** — Adicionar coluna `deletedAt` para exclusão lógica
- [ ] **Auditoria** — Colunas `criadoEm`, `atualizadoEm`, `criadoPor`
- [ ] **Carrinho de Compras** — Tabela para pedidos em rascunho
- [ ] **Wishlist** — Tabela para favoritos do cliente

---

## 📁 Estrutura do Repositório

```bash
ecommerce-db/
├── README.md                    # Este arquivo
├── diagrama-ecommerce.png       # Diagrama EER (exportado do Workbench)
├── ecommerce-db.mwb            # Arquivo do modelo MySQL Workbench
├── scripts/
│   ├── create-schema.sql        # Script para criar o banco
│   ├── insert-sample-data.sql   # Dados de exemplo
│   └── queries-uteis.sql        # Queries prontas para análise
└── .gitignore
```

---

## 🤝 Contribuições

Sugestões de melhorias são bem-vindas! Sinta-se livre para:
- Abrir issues com feedback
- Fazer fork e propor melhorias
- Compartilhar queries úteis

---

## 👤 Autor

**Gustavo Sampaio**

- 🎓 Tecnologia em Gestão da Tecnologia da Informação (GTI)
- 🔐 Especialista em Cybersecurity (em andamento)
- 🔗 [GitHub](https://github.com/guhsilva266)
- 🔗 [LinkedIn](https://www.linkedin.com/in/gustavo-sampaio/)

---

## 📄 Licença

Este projeto está disponível para fins educacionais e de portfólio.

---

## 📚 Referências

- [MySQL Documentation](https://dev.mysql.com/doc/)
- [MySQL Workbench Manual](https://dev.mysql.com/doc/workbench/en/)
- [Database Design Patterns](https://en.wikipedia.org/wiki/Database_design)
- [Entity-Relationship Model](https://en.wikipedia.org/wiki/Entity%E2%80%93relationship_model)

---
