# projetoIntegradoII

# UNIVERSIDADE FEDERAL DO CARIRI
## CENTRO DE EDUCAÇÃO A DISTÂNCIA
### ANÁLISE E DESENVOLVIMENTO DE SISTEMAS

## ENTREGÁVEL PARCIAL 1 (EP1): 
### PROJETO FÍSICO DE BANCO DE DADOS

### JUAZEIRO DO NORTE-CE
#### 2024

**Autores:**
- Darlan Almeida Barroso (2023011303)
- Francisco Jeferson da Silva Dantas (2023011706)
- Maria Welaine Dantas Angelo (2023011789)
- Paulo Gonçalo Farias Gonçalves (2023011798) 

**Docente:**
- Prof. Dr. Allysson Allex de Paula Araújo

---

### Sumário
1. [Apresentação](#1-apresentação)
2. [Descrição geral](#2-descrição-geral)
3. [Modelo lógico](#3-modelo-lógico)
4. [Modelo físico](#4-modelo-físico)

---

## 1 Apresentação

Este documento apresenta elementos que subsidiarão o desenvolvimento de um “sistema de estoque de um supermercado de bairro”, tema proposto na disciplina Projeto Integrado II, do curso de Análise e Desenvolvimento de Sistemas, da Universidade Federal do Cariri. O presente Projeto do Banco de Dados inclui os modelos, características e justificativas. Além disso, foi subdividido em cinco seções, são elas: apresentação, descrição geral (que discorre sobre o negócio e os setores envolvidos etc.), o modelo lógico e o modelo físico.

## 2 Descrição geral 

Dada a necessidade de prover níveis de acessos distintos para cada funcionário do supermercado, segundo o setor em que trabalham, será necessário criar uma entidade usuário. Considerando que o sistema visa acompanhar o fluxo dos produtos da compra até a venda ao consumidor, os setores (que se tornarão entidades) que irão utilizar o sistema são: setor de compras (responsável pelo contato com os fornecedores para compra dos produtos do supermercado), setor de depósito (responsável pelo recebimento do produto no depósito do supermercado), setor de reposição (responsável pela reposição do produto nas prateleiras do supermercado) e o setor de caixa (responsável por realizar a venda dos produtos selecionados pelo cliente e receber o pagamento).

Para a interação com os respectivos setores do supermercado, criamos ainda as seguintes entidades: fornecedor (ente externo responsável por prover o produto para o supermercado), cliente (indivíduo responsável pela compra do produto no supermercado), produto (entidade que apresenta detalhamentos sobre cada item vendido no supermercado) e prateleira (responsável por alocar o produto que está à disposição do cliente). 

Para melhor detalhar as informações supracitadas, apresentamos a Tabela 1 adiante, que apresenta o nome de cada tabela, os atributos, tipos de dados, tamanhos, limitações e descrições:

### Tabela 1: Detalhamento das entidades e algumas de suas características

#### Tabela: Usuário
| Atributos     | Tipo de Dados | Tamanho | Constraint   | Descrição                   |
|---------------|---------------|---------|--------------|-----------------------------|
| idUsuario     | INT           | 11      | PRIMARY KEY  | Identificador único do usuário |
| loginUsuario  | VARCHAR       | 50      | NOT NULL     | Login do usuário            |
| senhaUsuario  | VARCHAR       | 256     | NOT NULL     | Senha criptografada do usuário |
| setorUsuario  | VARCHAR       | 50      | NOT NULL     | Setor do usuário            |

#### Tabela: Caixa
| Atributos         | Tipo de Dados | Tamanho | Constraint   | Descrição                |
|-------------------|---------------|---------|--------------|--------------------------|
| idCaixa           | INT           | 11      | PRIMARY KEY  | Identificador único do caixa |
| idVenda           | INT           | -       | NOT NULL     |                          |
| idCliente         | INT           | 11      | FOREIGN KEY  | Referência ao cliente    |
| idProduto         | INT           | 11      | FOREIGN KEY  | Referência ao produto    |
| quantidadeVendida | INT           | -       | NOT NULL     | Quantidade de produtos vendidos |
| dataVenda         | DATETIME      | -       | NOT NULL     | Data da venda            |

#### Tabela: Cliente
| Atributos         | Tipo de Dados | Tamanho | Constraint   | Descrição                |
|-------------------|---------------|---------|--------------|--------------------------|
| idCliente         | INT           | 11      | PRIMARY KEY  | Identificador único do cliente |
| nome              | VARCHAR       | 100     | NOT NULL     | Nome do cliente          |
| contato           | VARCHAR       | 100     | NOT NULL     | Contato do cliente       |
| idProduto         | INT           | 11      | NOT NULL     |                          |
| idVenda           | INT           | 11      | NOT NULL     |                          |
| endereco          | VARCHAR       | 255     | NOT NULL     | Endereço do cliente      |
| quantidadeComprada| INT           | -       | NOT NULL     | Quantidade de produtos comprados |
| dataCompra        | DATETIME      | -       | NOT NULL     | Data da compra           |

#### Tabela: Produto
| Atributos     | Tipo de Dados | Tamanho | Constraint   | Descrição                |
|---------------|---------------|---------|--------------|--------------------------|
| idProduto     | INT           | 11      | PRIMARY KEY  | Identificador único do produto |
| nome          | VARCHAR       | 100     | NOT NULL     | Nome do produto          |
| precoCompra   | DECIMAL       | 10,2    | NOT NULL     | Preço de compra do produto |
| precoVenda    | DECIMAL       | 10,2    | NOT NULL     | Preço de venda do produto |
| codigoBarra   | INT           | -       | NOT NULL     | Código para leitura do produto |
| dataValidade  | DATE          | -       | NOT NULL     | Data de validade do produto |
| idFornecedor  | INT           | 11      | FOREIGN KEY  | Referência ao fornecedor  |

#### Tabela: Fornecedor
| Atributos     | Tipo de Dados | Tamanho | Constraint   | Descrição                |
|---------------|---------------|---------|--------------|--------------------------|
| idFornecedor  | INT           | 11      | PRIMARY KEY  | Identificador único do fornecedor |
| nome          | VARCHAR       | 100     | NOT NULL     | Nome do fornecedor       |
| endereco      | VARCHAR       | 255     | NOT NULL     | Endereço do fornecedor   |
| contato       | VARCHAR       | 100     | NOT NULL     | Contato do fornecedor    |

#### Tabela: Setor de Reposição
| Atributos     | Tipo de Dados | Tamanho | Constraint   | Descrição                |
|---------------|---------------|---------|--------------|--------------------------|
| idReposicao   | INT           | 11      | PRIMARY KEY  | Identificador único da reposição |
| idDeposito    | INT           | 11      | FOREIGN KEY  | Referência ao depósito   |
| idProduto     | INT           | 11      | FOREIGN KEY  | Referência ao produto    |
| quantidade    | INT           | -       | NOT NULL     | Quantidade de produtos repostos |
| dataValidade  | DATETIME      | -       | NOT NULL     | Data da validade         |
| dataReposicao | DATETIME      | -       | NOT NULL     | Data da reposição        |

#### Tabela: Depósito
| Atributos          | Tipo de Dados | Tamanho | Constraint   | Descrição                |
|--------------------|---------------|---------|--------------|--------------------------|
| idDeposito         | INT           | 11      | PRIMARY KEY  | Identificador único do depósito |
| idProduto          | INT           | 11      | FOREIGN KEY  | Referência ao produto    |
| quantidadeEmEstoque| INT           | -       | NOT NULL     | Quantidade de produtos em estoque |
| dataRecebimento    | DATETIME      | -       | NOT NULL     | Data de recebimento dos produtos |

#### Tabela: Setor de Compras
| Atributos     | Tipo de Dados | Tamanho | Constraint   | Descrição                |
|---------------|---------------|---------|--------------|--------------------------|
| idCompras     | INT           | 11      | PRIMARY KEY  | Identificador único da compra |
| idFornecedor  | INT           | 11      | FOREIGN KEY  | Referência ao fornecedor  |
| idProduto     | INT           | 11      | FOREIGN KEY  | Referência ao produto     |
| quantidade    | INT           | -       | NOT NULL     | Quantidade de produtos comprados |
| precoCompra   | DECIMAL       | 10,2    | NOT NULL     | Preço de compra do produto |

#### Tabela: Prateleira Supermercado
| Atributos     | Tipo de Dados | Tamanho | Constraint   | Descrição                |
|---------------|---------------|---------|--------------|--------------------------|
| idPrateleira  | INT           | 11      | PRIMARY KEY  | Identificador único da prateleira |
| idProduto     | INT           | 11      | FOREIGN KEY  | Referência ao produto    |
| precoVenda    | DECIMAL       | 10,2    | NOT NULL     | Preço de venda do produto |
| quantidade    | INT           | -       | NOT NULL     | Quantidade de produtos na prateleira |
| dataValidade  | DATETIME      | -       | NOT NULL     | Data da validade         |

Fonte: Elaborado pelos autores (2024).

### 2.1 Características Técnicas
- **SGBD Utilizado:** SQLite 3
- **Armazenamento:**
  - Tipo de Tabelas: SQLite armazena os dados em tabelas que suportam a integridade referencial através do uso de chaves estrangeiras.
  - Indexação: Índices primários em IDs e índices únicos em colunas críticas para garantir a eficiência nas consultas. Em SQLite, índices podem ser criados para melhorar a performance das consultas.
  - Transações: SQLite suporta transações ACID, garantindo que as operações de banco de dados sejam confiáveis e que a integridade dos dados seja mantida.
- **Backup e Recuperação:**
  - Backup: A cópia de segurança pode ser realizada copiando o arquivo de banco de dados (.db). Backup diário automatizado pode ser configurado utilizando scripts agendados (cron jobs no Unix ou tarefas agendadas no Windows).
  - Retenção de backups: Retenção de backups por 30 dias, utilizando políticas de rotação para gerenciar o armazenamento dos backups.
  - Recuperação de Desastres: A recuperação de desastres é testada mensalmente, restaurando os backups em um ambiente de teste para garantir que os dados possam ser recuperados com sucesso.

### 2.2 Justificativa das Decisões
- **Escolha do SGBD:** O SQLite foi escolhido devido à sua leveza, facilidade de uso e integração com aplicativos pequenos e médios. Ele oferece uma solução de banco de dados embutida que não requer um servidor separado, facilitando a implantação e o gerenciamento.
- **Tipo de Tabelas:** O uso de tabelas SQLite com suporte a transações ACID garante que as operações de banco de dados sejam confiáveis e que a integridade dos dados seja mantida. A integridade referencial é mantida através do uso de chaves estrangeiras.
- **Indexação:** A indexação foi configurada para melhorar a performance de consultas frequentes, especialmente em colunas de pesquisa e junção (e.g., idCliente, idProduto). Índices ajudam a acelerar as operações de leitura e junção de dados, essenciais para um desempenho eficiente.
- **Criptografia de Senhas:** A implementação de senha criptografada para garantir a segurança dos dados sensíveis dos usuários é essencial. A criptografia das senhas pode ser implementada utilizando bibliotecas de criptografia em Python, como bcrypt ou hashlib, antes de armazená-las no banco de dados.
- **Backup e Recuperação:** A estratégia de backup definida para assegurar a continuidade dos negócios e a recuperação de dados em caso de falhas é crucial. O uso de backups diários automatizados e a retenção de backups por 30 dias garantem que os dados possam ser restaurados em caso de perda ou corrupção. A recuperação de desastres é testada mensalmente para garantir a eficácia do processo de restauração.

## 3 Modelo lógico 

```sql
CREATE TABLE Usuario (
    idUsuario INT PRIMARY KEY AUTO_INCREMENT,
    loginUsuario VARCHAR(50) NOT NULL,
    senhaUsuario VARCHAR(256) NOT NULL,
    setorUsuario VARCHAR(50) NOT NULL
);

CREATE TABLE Cliente (
    idCliente INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    contato VARCHAR(100) NOT NULL,
    idProduto INT,
    idVenda INT,
    endereco VARCHAR(255) NOT NULL,
    quantidadeComprada INT NOT NULL,
    dataCompra DATETIME NOT NULL
);

CREATE TABLE Fornecedor (
    idFornecedor INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    endereco VARCHAR(255) NOT NULL,
    contato VARCHAR(100) NOT NULL
);

CREATE TABLE Produto (
    idProduto INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    precoCompra DECIMAL(10,2) NOT NULL,
    precoVenda DECIMAL(10,2) NOT NULL,
    codigoBarra INT,
    dataValidade DATE NOT NULL,
    idFornecedor INT,
    FOREIGN KEY (idFornecedor) REFERENCES Fornecedor(idFornecedor)
);

CREATE TABLE Caixa (
    idCaixa INT PRIMARY KEY AUTO_INCREMENT,
    idVenda INT,
    idProduto INT,
    idCliente INT,
    quantidadeVendida INT NOT NULL,
    dataVenda DATETIME NOT NULL,
    FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente),
    FOREIGN KEY (idProduto) REFERENCES Produto(idProduto)
);

CREATE TABLE Deposito (
    idDeposito INT PRIMARY KEY AUTO_INCREMENT,
    idProduto INT,
    quantidadeEmEstoque INT NOT NULL,
    dataRecebimento DATETIME NOT NULL,
    FOREIGN KEY (idProduto) REFERENCES Produto(idProduto)
);

CREATE TABLE SetorReposicao (
    idReposicao INT PRIMARY KEY AUTO_INCREMENT,
    idDeposito INT,
    quantidade INT NOT NULL,
    idProduto INT,
    dataValidade DATETIME NOT NULL,
    dataReposicao DATETIME NOT NULL,
    FOREIGN KEY (idDeposito) REFERENCES Deposito(idDeposito),
    FOREIGN KEY (idProduto) REFERENCES Produto(idProduto)
);

CREATE TABLE SetorCompras (
    idCompras INT PRIMARY KEY AUTO_INCREMENT,
    idFornecedor INT,
    idProduto INT,
    quantidade INT NOT NULL,
    precoCompra DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (idFornecedor) REFERENCES Fornecedor(idFornecedor),
    FOREIGN KEY (idProduto) REFERENCES Produto(idProduto)
);

CREATE TABLE PrateleiraSupermercado (
    idPrateleira INT PRIMARY KEY AUTO_INCREMENT,
    idProduto INT,
    precoVenda DECIMAL(10,2) NOT NULL,
    quantidade INT NOT NULL,
    dataValidade DATETIME NOT NULL,
    FOREIGN KEY (idProduto) REFERENCES Produto(idProduto)
);
