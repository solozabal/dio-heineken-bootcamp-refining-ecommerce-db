# Order Management System

This project is an order management system that allows you to manage products, suppliers, customers, orders, order items, payments and deliveries.

## Entidades e Atributos

### Produto
- **ID_Produto** (chave primária)
- **Nome**
- **Descrição**
- **Preço**
- **Quantidade_Em_Estoque**
- **ID_Fornecedor** (chave estrangeira, referenciando Fornecedor)

### Fornecedor
- **ID_Fornecedor** (chave primária)
- **Nome**
- **CNPJ**
- **Endereço**
- **Telefone**
- **Email**

### Cliente
- **ID_Cliente** (chave primária)
- **Nome**
- **Tipo_Cliente** (enum: 'PF', 'PJ') // Identifica se o cliente é pessoa física ou jurídica
- **CPF** (campo único, obrigatório se Tipo_Cliente = 'PF')
- **CNPJ** (campo único, obrigatório se Tipo_Cliente = 'PJ')
- **Endereço**
- **Telefone**
- **Email**

### Pedido
- **ID_Pedido** (chave primária)
- **ID_Cliente** (chave estrangeira, referenciando Cliente)
- **Data_Pedido**
- **Status** (ex: Pendente, Enviado, Cancelado)
- **Endereco_Entrega**
- **Valor_Frete**
- **Status_Entrega** (ex: Pendente, Em Trânsito, Entregue) // Novo atributo
- **Codigo_Rastreio** // Novo atributo

### Item_Pedido
- **ID_Item_Pedido** (chave primária)
- **ID_Pedido** (chave estrangeira, referenciando Pedido)
- **ID_Produto** (chave estrangeira, referenciando Produto)
- **Quantidade**

### Pagamento
- **ID_Pagamento** (chave primária)
- **ID_Pedido** (chave estrangeira, referenciando Pedido)
- **Tipo_Pagamento** (ex: Cartão de Crédito, Boleto, Transferência)
- **Data_Pagamento**
- **Valor_Pago**

### Entrega (opcional)
- **ID_Entrega** (chave primária)
- **ID_Pedido** (chave estrangeira, referenciando Pedido)
- **Status_Entrega** (ex: Pendente, Em Trânsito, Entregue)
- **Codigo_Rastreio**

## Relacionamentos
- Um fornecedor pode fornecer muitos produtos; cada produto é fornecido por apenas um fornecedor (um-para-muitos).
- Um cliente pode fazer múltiplos pedidos (um-para-muitos).
- Um pedido pode conter múltiplos produtos; um produto pode estar em múltiplos pedidos (muitos-para-muitos).
- Um pedido pode ter múltiplas formas de pagamento associadas.
- Cada pedido pode ter uma ou mais entregas associadas.

## Diagrama UML

```mermaid
classDiagram
    class Produto {
        +ID_Produto: int <<PK>>
        +Nome: string
        +Descrição: string
        +Preço: decimal
        +Quantidade_Em_Estoque: int
        +ID_Fornecedor: int <<FK>>
    }

    class Fornecedor {
        +ID_Fornecedor: int <<PK>>
        +Nome: string
        +CNPJ: string
        +Endereço: string
        +Telefone: string
        +Email: string
    }

    class Cliente {
        +ID_Cliente: int <<PK>>
        +Nome: string
        +Tipo_Cliente: enum <<PF, PJ>>
        +CPF: string <<unique, FK>>
        +CNPJ: string <<unique, FK>>
        +Endereço: string
        +Telefone: string
        +Email: string
    }

    class Pedido {
        +ID_Pedido: int <<PK>>
        +ID_Cliente: int <<FK>>
        +Data_Pedido: date
        +Status: enum <<Pendente, Enviado, Cancelado>>
        +Endereco_Entrega: string
        +Valor_Frete: decimal
        +Status_Entrega: enum <<Pendente, Em Trânsito, Entregue>>
        +Codigo_Rastreio: string
    }

    class Item_Pedido {
        +ID_Item_Pedido: int <<PK>>
        +ID_Pedido: int <<FK>>
        +ID_Produto: int <<FK>>
        +Quantidade: int
    }

    class Pagamento {
        +ID_Pagamento: int <<PK>>
        +ID_Pedido: int <<FK>>
        +Tipo_Pagamento: enum <<Cartão de Crédito, Boleto, Transferência>>
        +Data_Pagamento: date
        +Valor_Pago: decimal
    }

    class Entrega {
        +ID_Entrega: int <<PK>>
        +ID_Pedido: int <<FK>>
        +Status_Entrega: enum <<Pendente, Em Trânsito, Entregue>>
        +Codigo_Rastreio: string
    }

    %% Relacionamentos %%
    Fornecedor "1" -- "0..*" Produto : fornece >
    Cliente "1" -- "0..*" Pedido : faz >
    Pedido "1" -- "0..*" Item_Pedido : contém >
    Produto "1" -- "0..*" Item_Pedido : está em >
    Pedido "1" -- "0..*" Pagamento : tem >
    Pedido "1" -- "0..*" Entrega : possui >