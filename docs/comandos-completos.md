# 📘 Guia Completo de Comandos SUI

## 🔧 Comandos de Configuração

### `sui client new-env`
Cria um novo ambiente de rede.

```bash
sui client new-env --alias <nome> --rpc <url-rpc>
```

**Exemplos:**
```bash
# DevNet
sui client new-env --alias devnet --rpc https://fullnode.devnet.sui.io:443

# TestNet
sui client new-env --alias testnet --rpc https://fullnode.testnet.sui.io:443

# MainNet
sui client new-env --alias mainnet --rpc https://fullnode.mainnet.sui.io:443
```

### `sui client envs`
Lista todos os ambientes configurados.

```bash
sui client envs
```

**Saída esperada:**
```
╭─────────┬─────────────────────────────────────┬────────╮
│ alias   │ url                                 │ active │
├─────────┼─────────────────────────────────────┼────────┤
│ devnet  │ https://fullnode.devnet.sui.io:443  │ *      │
│ testnet │ https://fullnode.testnet.sui.io:443 │        │
╰─────────┴─────────────────────────────────────┴────────╯
```

### `sui client switch`
Alterna entre ambientes.

```bash
sui client switch --env <nome-ambiente>
```

**Exemplo:**
```bash
sui client switch --env devnet
```

## 💰 Comandos de Carteira

### `sui client active-address`
Mostra o endereço ativo atual.

```bash
sui client active-address
```

**Saída esperada:**
```
0x1234567890abcdef1234567890abcdef12345678
```

### `sui client balance`
Exibe o saldo da carteira ativa.

```bash
sui client balance
```

**Saída esperada:**
```
╭────────────────────────────────────────╮
│ Balance of coins owned by this address │
├────────────────────────────────────────┤
│ ╭──────────────────────────────────────────────────────────────────────╮ │
│ │ coin  balance (raw)  balance (display)  coin type                     │ │
│ ├──────────────────────────────────────────────────────────────────────┤ │
│ │ SUI   1000000000    1.00 SUI           0x2::sui::SUI                  │ │
│ ╰──────────────────────────────────────────────────────────────────────╯ │
╰────────────────────────────────────────╯
```

### `sui client gas`
Lista objetos de gás disponíveis.

```bash
sui client gas
```

**Saída esperada:**
```
╭────────────────────────────────────────────────────────────────────╮
│ gasCoinId                          │ gasBalance │ suiBalance       │
├────────────────────────────────────────────────────────────────────┤
│ 0x123...abc                        │ 1000000000 │ 1000000000       │
╰────────────────────────────────────────────────────────────────────╯
```

### `sui client faucet`
Solicita tokens de teste (apenas redes de teste).

```bash
sui client faucet
```

**⚠️ Importante**: 
- Funciona apenas em DevNet e TestNet
- Limitado a uma solicitação por tempo
- Fornece tokens SUI para testes

## 🏗️ Comandos Move

### `sui move new`
Cria um novo projeto Move.

```bash
sui move new <nome-do-projeto>
```

**Exemplo:**
```bash
sui move new meu_primeiro_contrato
```

**Estrutura criada:**
```
meu_primeiro_contrato/
├── Move.toml
├── sources/
│   └── (seus arquivos .move aqui)
└── tests/
    └── (seus testes aqui)
```

### `sui move build`
Compila o projeto Move.

```bash
cd <nome-do-projeto>
sui move build
```

### `sui move test`
Executa os testes do projeto.

```bash
sui move test
```

## 🔍 Comandos de Consulta

### `sui client object`
Consulta informações sobre um objeto específico.

```bash
sui client object <object-id>
```

### `sui client ptb`
Executa um bloco de transação programável.

```bash
sui client ptb --help
```

## 📊 Comandos de Monitoramento

### `sui client active-env`
Mostra o ambiente ativo atual.

```bash
sui client active-env
```

### `sui client addresses`
Lista todos os endereços da carteira.

```bash
sui client addresses
```

## 🔐 Comandos de Segurança

### `sui keytool`
Gerencia chaves criptográficas.

```bash
sui keytool --help
```

### `sui client new-address`
Gera um novo endereço.

```bash
sui client new-address ed25519
```

---

## 💡 Dicas Importantes

1. **Sempre verifique o ambiente ativo** antes de executar comandos
2. **Use DevNet para desenvolvimento** e testes
3. **Mantenha suas chaves privadas seguras**
4. **Faça backup das suas configurações**

## 🚨 Comandos de Emergência

```bash
# Resetar configuração (cuidado!)
rm -rf ~/.sui

# Verificar status da rede
sui client call --help
```