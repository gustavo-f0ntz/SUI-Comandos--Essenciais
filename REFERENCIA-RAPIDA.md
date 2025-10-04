# ⚡ SUI - Referência Rápida

## 🚀 Setup Inicial (Copy & Paste)

```bash
# Configurar DevNet
sui client new-env --alias devnet --rpc https://fullnode.devnet.sui.io:443
sui client switch --env devnet
sui client faucet
sui client balance
```

## 💰 Comandos Essenciais

| Comando | Descrição |
|---------|-----------|
| `sui client active-address` | Mostra endereço ativo |
| `sui client balance` | Mostra saldo da carteira |
| `sui client gas` | Lista objetos de gás |
| `sui client faucet` | Solicita tokens de teste |
| `sui client envs` | Lista ambientes |
| `sui client switch --env devnet` | Muda para DevNet |

## 🏗️ Desenvolvimento Move

```bash
# Criar projeto
sui move new meu_projeto
cd meu_projeto

# Compilar
sui move build

# Testar
sui move test

# Publicar
sui client publish --gas-budget 20000000
```

## 🌐 URLs Importantes

| Rede | RPC URL |
|------|---------|
| **DevNet** | `https://fullnode.devnet.sui.io:443` |
| **TestNet** | `https://fullnode.testnet.sui.io:443` |
| **MainNet** | `https://fullnode.mainnet.sui.io:443` |

## 🔧 Troubleshooting Rápido

```bash
# Verificar tudo
sui client active-env
sui client active-address
sui client balance

# Reset se necessário
sui client switch --env devnet
sui client faucet
```

## 📚 Links Rápidos

- 📖 [Docs Oficiais](https://docs.sui.io/)
- 💬 [Discord](https://discord.gg/sui)
- 🔍 [Explorer](https://explorer.sui.io/)
- 🎮 [Playground](https://play.sui.io/)

---
**💡 Dica**: Sempre use DevNet para desenvolvimento e testes!