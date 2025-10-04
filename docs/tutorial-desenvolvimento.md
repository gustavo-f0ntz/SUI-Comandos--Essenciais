# 🔨 Tutorial de Desenvolvimento SUI

## 🎯 Objetivo
Este tutorial vai te guiar desde a configuração inicial até a criação do seu primeiro smart contract na SUI.

## 📋 Pré-requisitos
- SUI CLI instalado
- Conhecimento básico de linha de comando
- Editor de código (VS Code recomendado)

## 🚀 Passo a Passo

### 1. Configuração Inicial

#### 1.1 Verificar Instalação
```bash
sui --version
```

#### 1.2 Configurar DevNet
```bash
# Criar ambiente DevNet
sui client new-env --alias devnet --rpc https://fullnode.devnet.sui.io:443

# Verificar se foi criado
sui client envs

# Ativar DevNet
sui client switch --env devnet
```

### 2. Configuração da Carteira

#### 2.1 Verificar Endereço
```bash
sui client active-address
```

#### 2.2 Obter Tokens de Teste
```bash
sui client faucet
```

#### 2.3 Verificar Saldo
```bash
sui client balance
```

**Resultado esperado:**
```
Balance of coins owned by this address
╭──────────────────────────────────────────────────────────────────────╮
│ coin  balance (raw)  balance (display)  coin type                     │
├──────────────────────────────────────────────────────────────────────┤
│ SUI   1000000000    1.00 SUI           0x2::sui::SUI                  │
╰──────────────────────────────────────────────────────────────────────╯
```

### 3. Primeiro Projeto Move

#### 3.1 Criar Projeto
```bash
sui move new hello_world
cd hello_world
```

#### 3.2 Estrutura do Projeto
```
hello_world/
├── Move.toml
├── sources/
└── tests/
```

#### 3.3 Configurar Move.toml
```toml
[package]
name = "hello_world"
version = "0.0.1"
edition = "2024.beta"

[dependencies]
Sui = { git = "https://github.com/MystenLabs/sui.git", subdir = "crates/sui-framework/packages/sui-framework", rev = "framework/devnet" }

[addresses]
hello_world = "0x0"
```

#### 3.4 Criar Primeiro Módulo
Crie o arquivo `sources/hello_world.move`:

```move
module hello_world::hello_world {
    use sui::object::{Self, UID};
    use sui::transfer;
    use sui::tx_context::{Self, TxContext};
    use std::string;

    /// Uma estrutura simples que contém uma mensagem
    public struct HelloWorldObject has key, store {
        id: UID,
        text: string::String
    }

    /// Função pública para criar um novo HelloWorldObject
    public fun mint(ctx: &mut TxContext) {
        let object = HelloWorldObject {
            id: object::new(ctx),
            text: string::utf8(b"Hello World!")
        };
        transfer::public_transfer(object, tx_context::sender(ctx));
    }
}
```

### 4. Compilação e Teste

#### 4.1 Compilar o Projeto
```bash
sui move build
```

**Saída esperada:**
```
BUILDING hello_world
Build Successful
```

#### 4.2 Criar Teste
Crie o arquivo `tests/hello_world_tests.move`:

```move
#[test_only]
module hello_world::hello_world_tests {
    use hello_world::hello_world::{Self, HelloWorldObject};
    use sui::test_scenario;

    #[test]
    public fun test_mint() {
        let addr1 = @0xA;
        
        let mut scenario = test_scenario::begin(addr1);
        {
            hello_world::mint(test_scenario::ctx(&mut scenario));
        };
        
        test_scenario::next_tx(&mut scenario, addr1);
        {
            assert!(test_scenario::has_most_recent_for_sender<HelloWorldObject>(&scenario), 0);
        };
        
        test_scenario::end(scenario);
    }
}
```

#### 4.3 Executar Testes
```bash
sui move test
```

### 5. Deploy do Contrato

#### 5.1 Publicar o Módulo
```bash
sui client publish --gas-budget 20000000
```

#### 5.2 Chamar a Função
Após o deploy, use o Package ID retornado:

```bash
sui client call --function mint --module hello_world --package <PACKAGE_ID> --gas-budget 10000000
```

### 6. Verificação

#### 6.1 Verificar Objetos
```bash
sui client objects
```

#### 6.2 Inspecionar Objeto
```bash
sui client object <OBJECT_ID>
```

## 🎉 Parabéns!

Você criou e deployou seu primeiro smart contract na SUI! 

## 🔄 Próximos Passos

1. **Explore mais funcionalidades Move**
2. **Crie contratos mais complexos**
3. **Aprenda sobre capabilities**
4. **Estude o sistema de objetos da SUI**

## 📚 Recursos para Continuar

- [Documentação oficial Move](https://move-language.github.io/move/)
- [SUI Developer Portal](https://docs.sui.io/)
- [Exemplos de código](https://github.com/MystenLabs/sui/tree/main/examples)

## 🐛 Problemas Comuns

### Erro de Gás Insuficiente
```bash
# Verificar gás disponível
sui client gas

# Solicitar mais tokens
sui client faucet
```

### Erro de Compilação
```bash
# Verificar sintaxe do Move.toml
# Verificar importações
# Consultar logs de erro detalhados
```

### Ambiente Incorreto
```bash
# Verificar ambiente ativo
sui client active-env

# Alternar se necessário
sui client switch --env devnet
```