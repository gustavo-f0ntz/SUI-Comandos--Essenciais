# 🎯 Exemplos Práticos SUI

## 📋 Lista de Exemplos

### Básicos
- [Hello World](#hello-world)
- [TodoList - Lista de Tarefas](#todolist---lista-de-tarefas)
- [Contador Simples](#contador-simples)

### Intermediários
- [NFT com Display - Exemplo Real](#nft-com-display---exemplo-real)
- [Token Personalizado](#token-personalizado)
- [Marketplace Simples](#marketplace-simples)

---

## TodoList - Lista de Tarefas

> 📝 **Exemplo prático baseado no projeto real**: [move-smart-todolist](https://github.com/gustavo-f0ntz/move-smart-todolist)

### Move.toml
```toml
[package]
name = "todo_list"
version = "0.0.1"
edition = "2024.beta"

[dependencies]
Sui = { git = "https://github.com/MystenLabs/sui.git", subdir = "crates/sui-framework/packages/sui-framework", rev = "framework/devnet" }

[addresses]
todo_list = "0x0"
```

### sources/todo_list.move
```move
/// Module: todo_list
module todo_list::todo_list;

use std::string::String;

/// List of todos. Can be managed by owner and shared with others.
public struct TodoList has key, store {
    id: UID,
    items: vector<String>
}

/// Create a new todo list.
public fun new(ctx: &mut TxContext): TodoList {
    let list = TodoList {
        id: object::new(ctx),
        items: vector[]
    };

    (list)
}

/// Add a new item to the todo list.
public fun add(list: &mut TodoList, item: String) {
    list.items.push_back(item);
}

/// Remove a todo item from the list by index.
public fun remove(list: &mut TodoList, index: u64): String {
    list.items.remove(index)
}

/// Delete the list and capability to manage it.
public fun delete(list: TodoList) {
    let TodoList {id, items: _ } = list;
    id.delete();
}

/// Get the number of items in the list.
public fun length(list: &TodoList): u64 {
    list.items.length()
}
```

### Como Usar o TodoList
```bash
# 1. Criar projeto
sui move new todo_list
cd todo_list

# 2. Copiar código acima nos arquivos

# 3. Compilar e publicar
sui move build
sui client publish --gas-budget 20000000

# 4. Criar uma nova lista (usar PACKAGE_ID do deploy)
sui client call --function new --module todo_list --package <PACKAGE_ID> --gas-budget 10000000

# 5. Adicionar item na lista (usar OBJECT_ID da lista criada)
sui client call --function add --module todo_list --package <PACKAGE_ID> --args <OBJECT_ID> "Estudar SUI" --gas-budget 10000000

# 6. Verificar lista
sui client object <OBJECT_ID>

# 7. Remover item (índice 0)
sui client call --function remove --module todo_list --package <PACKAGE_ID> --args <OBJECT_ID> 0 --gas-budget 10000000
```

### Funcionalidades do TodoList
- ✅ **Criar lista** - `new()`
- ✅ **Adicionar tarefa** - `add()`
- ✅ **Remover tarefa** - `remove()`
- ✅ **Verificar tamanho** - `length()`
- ✅ **Deletar lista** - `delete()`

### Conceitos Aprendidos
- 📦 **Structs com capabilities** (`key`, `store`)
- 🔧 **Manipulação de vectors**
- 🎯 **Gestão de objetos** (criar, modificar, deletar)
- 📝 **Strings em Move**
- 🔑 **UIDs para identificação única**

---

## Hello World

### Move.toml
```toml
[package]
name = "introducao"
version = "0.0.1"
edition = "2024.beta"

[dependencies]
Sui = { git = "https://github.com/MystenLabs/sui.git", subdir = "crates/sui-framework/packages/sui-framework", rev = "framework/devnet" }

[addresses]
introducao = "0x0"
```

### sources/pratica_sui.move
```move
module introducao::pratica_sui {
    use std::debug::print;
    use std::string::utf8;

    fun pratica() {
        print(&utf8(b"Hello, World!"));
    }

    #[test]
    fun teste() {
        pratica();
    }
}
```

### Como Usar
```bash
# 1. Criar projeto
sui move new introducao
cd introducao

# 2. Copiar código acima nos arquivos

# 3. Executar o teste para ver a saída
sui move test

# Saída esperada:
# [debug] "Hello, World!"
```

### 🎯 Desafio para Praticar

**Modifique o código para:**

1. **Criar uma nova função de saudação**:
```move
fun saudacao_personalizada() {
    print(&utf8(b"Olá, SUI Blockchain!"));
}
```

2. **Adicionar função com parâmetro**:
```move
fun cumprimentar(nome: vector<u8>) {
    let mensagem = utf8(b"Olá, ");
    // Desafio: Como concatenar o nome na mensagem?
}
```

3. **Criar múltiplas saudações**:
```move
fun multiplas_saudacoes() {
    print(&utf8(b"Primeira saudação!"));
    print(&utf8(b"Segunda saudação!"));
    print(&utf8(b"Terceira saudação!"));
}
```

### Conceitos Aprendidos
- 📝 **Debug print** - Como imprimir valores para debug
- 🔤 **Strings** - Usando `utf8()` para criar strings
- 🧪 **Tests** - Como criar e executar testes
- 📦 **Módulos** - Estrutura básica de um módulo Move

### 💡 Dicas
- Use `sui move test` para executar e ver as saídas
- Experimente diferentes mensagens
- Tente criar suas próprias funções de saudação

---

## Contador Simples

### sources/counter.move
```move
module counter::counter {
    use sui::object::{Self, UID};
    use sui::transfer;
    use sui::tx_context::{Self, TxContext};

    public struct Counter has key {
        id: UID,
        value: u64,
    }

    public fun create_counter(ctx: &mut TxContext) {
        let counter = Counter {
            id: object::new(ctx),
            value: 0,
        };
        transfer::share_object(counter);
    }

    public fun increment(counter: &mut Counter) {
        counter.value = counter.value + 1;
    }

    public fun decrement(counter: &mut Counter) {
        if (counter.value > 0) {
            counter.value = counter.value - 1;
        };
    }

    public fun get_value(counter: &Counter): u64 {
        counter.value
    }

    public fun reset(counter: &mut Counter) {
        counter.value = 0;
    }
}
```

### Como Usar
```bash
# 1. Publicar
sui client publish --gas-budget 20000000

# 2. Criar contador (usar PACKAGE_ID do deploy)
sui client call --function create_counter --module counter --package <PACKAGE_ID> --gas-budget 10000000

# 3. Incrementar (usar OBJECT_ID do contador criado)
sui client call --function increment --module counter --package <PACKAGE_ID> --args <OBJECT_ID> --gas-budget 10000000
```

---

## NFT com Display - Exemplo Real

> 🎨 **Exemplo prático baseado no projeto real**: [sui-nft-create](https://github.com/gustavo-f0ntz/sui-nft-create/tree/master)

### Move.toml
```toml
[package]
name = "meu_nft_exemplo"
version = "0.0.1"
edition = "2024.beta"

[dependencies]
Sui = { git = "https://github.com/MystenLabs/sui.git", subdir = "crates/sui-framework/packages/sui-framework", rev = "framework/devnet" }

[addresses]
meu_nft_exemplo = "0x0"
```

### sources/meu_nft.move
```move
/// Módulo para criar um NFT de exemplo com Display Padrão.
module meu_nft_exemplo::meu_nft {
    // --- Dependências ---
    // Importações necessárias do framework da Sui.

    // Para criar o Display que mostra nosso NFT em UIs.
    use sui::display;
    // Para usar o tipo String.
    use std::string::{Self, String};
    // Para poder pegar o Publisher na inicialização.
    use sui::package::{Self, Publisher};

    // --- Definição do Objeto NFT ---

    /// One-Time Witness para o Publisher
    public struct MEU_NFT has drop {}

    /// A estrutura principal do nosso NFT.
    /// 'has key' permite que ele seja um objeto que pode ser possuído.
    /// 'has store' permite que ele seja colocado dentro de outras estruturas.
    public struct MeuNFT has key, store {
        id: UID,
        name: String,
        description: String,
        /// URL para a imagem do NFT (idealmente, um link de gateway IPFS https://).
        url: String
    }

    // --- Funções de Inicialização ---

    /// Esta função é chamada apenas UMA VEZ, quando o módulo é publicado na rede.
    /// Ela cria o "Publisher", que é um objeto que nos dá permissão para criar
    /// e atualizar o Display do nosso tipo `MeuNFT`.
    fun init(otw: MEU_NFT, ctx: &mut TxContext) {
        // Cria um novo objeto Publisher para o nosso tipo `MeuNFT`.
        let publisher = package::claim(otw, ctx);
        // Transfere o Publisher para a pessoa que está publicando o contrato.
        // Isso garante que apenas o criador do contrato possa mudar como os NFTs são exibidos.
        transfer::public_transfer(publisher, tx_context::sender(ctx));
    }

    // --- Funções Públicas (Entry Functions) ---

    /// Cria ("minta") uma nova instância do nosso NFT e a envia para o chamador.
    entry fun mint(
        name: vector<u8>,
        description: vector<u8>,
        url: vector<u8>,
        ctx: &mut TxContext
    ) {
        // Cria o objeto NFT com os dados fornecidos.
        let nft = MeuNFT {
            id: object::new(ctx),
            name: string::utf8(name),
            description: string::utf8(description),
            url: string::utf8(url),
        };
        // Transfere o NFT recém-criado para a carteira que chamou esta função.
        transfer::public_transfer(nft, tx_context::sender(ctx));
    }

    /// Cria e publica o objeto `Display` para o tipo `MeuNFT`.
    /// Esta função deve ser chamada APENAS UMA VEZ após a publicação do contrato.
    /// Requer o objeto `Publisher` (que foi obtido no `init`) como prova de autoridade.
    entry fun create_display(
        publisher: &Publisher,
        ctx: &mut TxContext
    ) {
        // Cria um novo objeto Display.
        let mut display = display::new_with_fields<MeuNFT>(
            publisher,
            // Nomes dos campos que aparecerão no Display.
            // Estes são os nomes que as carteiras e exploradores vão ler.
            vector[
                string::utf8(b"name"),
                string::utf8(b"description"),
                string::utf8(b"image_url") // Nome padrão para a imagem!
            ],
            // Valores para os campos do Display.
            // Usamos "placeholders" (variáveis) que apontam para os campos do nosso struct `MeuNFT`.
            // "{name}" no display vai mostrar o valor do campo "name" do NFT.
            // "{url}" no display vai mostrar o valor do campo "url" do NFT.
            vector[
                string::utf8(b"{name}"),
                string::utf8(b"{description}"),
                string::utf8(b"{url}") // Mapeamos nosso campo 'url' para o 'image_url' do display!
            ],
            ctx
        );

        // Atualiza a versão do display para torná-lo ativo.
        display::update_version(&mut display);
        // Transfere o objeto Display para o sender.
        transfer::public_transfer(display, tx_context::sender(ctx));
    }
}
```

### Como Usar o NFT
```bash
# 1. Criar projeto
sui move new meu_nft_exemplo
cd meu_nft_exemplo

# 2. Copiar código acima nos arquivos

# 3. Compilar e publicar
sui move build
sui client publish --gas-budget 20000000

# 4. Passo 1: Criar o Display (usar PACKAGE_ID e PUBLISHER_ID do deploy)
sui client call --function create_display --module meu_nft --package <PACKAGE_ID> --args <PUBLISHER_ID> --gas-budget 10000000

# 5. Passo 2: Mintar um NFT
sui client call --function mint --module meu_nft --package <PACKAGE_ID> --args "Meu Primeiro NFT" "Um NFT incrível criado na SUI!" "https://example.com/image.png" --gas-budget 10000000

# 6. Verificar NFT criado
sui client object <NFT_OBJECT_ID>
```

### Funcionalidades do NFT
- 🎨 **Criar NFT** - `mint()` com nome, descrição e imagem
- 📱 **Display padrão** - Aparece corretamente em carteiras e exploradores
- 🔐 **Publisher control** - Apenas o criador pode modificar o display
- 🖼️ **Suporte a imagens** - URLs para imagens (IPFS recomendado)

### Conceitos Avançados Aprendidos
- 🏗️ **One-Time Witness (OTW)** - Padrão para inicialização única
- 📺 **Display Object** - Como NFTs aparecem em UIs
- 🔑 **Publisher** - Controle de autoridade sobre tipos
- 📋 **Entry functions** - Funções chamáveis diretamente
- 🔄 **Init function** - Função executada no deploy

### 💡 Dicas Importantes
- **Publisher**: Guarde bem o objeto Publisher, é sua autoridade!
- **Display**: Crie apenas uma vez após o deploy
- **URLs**: Use IPFS para imagens descentralizadas
- **Metadados**: O Display mapeia campos para padrões de carteira

---

## Token Personalizado

### sources/my_coin.move
```move
module my_coin::my_coin {
    use sui::coin::{Self, Coin, TreasuryCap};
    use sui::url::{Self, Url};

    public struct MY_COIN has drop {}

    fun init(witness: MY_COIN, ctx: &mut TxContext) {
        let (treasury_cap, metadata) = coin::create_currency<MY_COIN>(
            witness,
            6,                // decimals
            b"MYCOIN",        // symbol
            b"My Coin",       // name
            b"A simple educational coin", // description
            option::some<Url>(url::new_unsafe_from_bytes(b"https://example.com/icon.png")), // icon
            ctx
        );
        
        transfer::public_freeze_object(metadata);
        transfer::public_transfer(treasury_cap, tx_context::sender(ctx))
    }

    public fun mint(
        treasury_cap: &mut TreasuryCap<MY_COIN>, 
        amount: u64, 
        recipient: address, 
        ctx: &mut TxContext
    ) {
        coin::mint_and_transfer(treasury_cap, amount, recipient, ctx)
    }

    public fun burn(treasury_cap: &mut TreasuryCap<MY_COIN>, coin: Coin<MY_COIN>) {
        coin::burn(treasury_cap, coin);
    }
}
```

---

## Marketplace Simples

### sources/marketplace.move
```move
module marketplace::marketplace {
    use sui::object::{Self, UID, ID};
    use sui::transfer;
    use sui::tx_context::{Self, TxContext};
    use sui::coin::{Self, Coin};
    use sui::sui::SUI;
    use sui::dynamic_object_field as dof;

    public struct Marketplace has key {
        id: UID,
    }

    public struct Listing has key, store {
        id: UID,
        item_id: ID,
        ask: u64,
        owner: address,
    }

    fun init(ctx: &mut TxContext) {
        let marketplace = Marketplace {
            id: object::new(ctx),
        };
        transfer::share_object(marketplace);
    }

    public fun list_item<T: key + store>(
        marketplace: &mut Marketplace,
        item: T,
        ask: u64,
        ctx: &mut TxContext
    ) {
        let item_id = object::id(&item);
        let listing = Listing {
            id: object::new(ctx),
            item_id,
            ask,
            owner: tx_context::sender(ctx),
        };
        
        dof::add(&mut marketplace.id, item_id, item);
        transfer::public_transfer(listing, tx_context::sender(ctx));
    }

    public fun purchase<T: key + store>(
        marketplace: &mut Marketplace,
        listing: Listing,
        payment: Coin<SUI>,
        ctx: &mut TxContext
    ): T {
        let Listing { id, item_id, ask, owner } = listing;
        
        assert!(coin::value(&payment) >= ask, 0);
        
        let item: T = dof::remove(&mut marketplace.id, item_id);
        
        transfer::public_transfer(payment, owner);
        object::delete(id);
        
        item
    }
}
```

---

## 🚀 Como Usar os Exemplos

### 1. Preparação
```bash
# Configurar ambiente
sui client switch --env devnet
sui client faucet
```

### 2. Para cada exemplo
```bash
# Criar projeto
sui move new <nome_exemplo>
cd <nome_exemplo>

# Copiar código
# Editar Move.toml e arquivos .move

# Compilar e testar
sui move build
sui move test

# Publicar
sui client publish --gas-budget 20000000
```

### 3. Interagir
```bash
# Usar PACKAGE_ID retornado no deploy
sui client call --function <função> --module <módulo> --package <PACKAGE_ID> --args <argumentos> --gas-budget 10000000
```

---

## 📚 Próximos Passos

1. **Comece com Hello World** - Entenda a estrutura básica
2. **Pratique com TodoList** - Aprenda manipulação de dados
3. **Explore o Contador** - Veja objetos compartilhados
4. **Crie suas variações** - Modifique os exemplos
5. **Combine conceitos** - Misture diferentes funcionalidades

### 💡 Projetos Sugeridos
- **TodoList com categorias** - Adicione tipos de tarefa
- **Contador com múltiplos usuários** - Sistema de pontuação
- **Hello World personalizado** - Mensagens customizáveis

## 🔗 Links Úteis

### 🎯 Repositórios dos Exemplos
- [📝 TodoList Original](https://github.com/gustavo-f0ntz/move-smart-todolist)
- [🎨 NFT com Display](https://github.com/gustavo-f0ntz/sui-nft-create/tree/master)

### 📚 Recursos Oficiais
- [📚 Mais exemplos oficiais](https://github.com/MystenLabs/sui/tree/main/examples/move)
- [🎓 Move Book](https://move-language.github.io/move/)
- [🌟 SUI by Example](https://examples.sui.io/)