# 🚀 SUI - Comandos Essenciais

Documentação completa dos comandos essenciais para desenvolvimento na blockchain SUI.

## 📋 Índice

- [🔧 Configuração Inicial](#-configuração-inicial)
- [💰 Gerenciamento de Carteira](#-gerenciamento-de-carteira)
- [🌐 Ambientes de Rede](#-ambientes-de-rede)
- [🏗️ Desenvolvimento Move](#️-desenvolvimento-move)
- [📖 Guias Detalhados](#-guias-detalhados)

## 🔧 Configuração Inicial

### Verificar Instalação
```bash
sui --version
```

### Configurar Ambiente de Desenvolvimento
```bash
# Criar novo ambiente DevNet
sui client new-env --alias devnet --rpc https://fullnode.devnet.sui.io:443

# Verificar ambientes disponíveis
sui client envs

# Alternar para DevNet
sui client switch --env devnet
```

## 💰 Gerenciamento de Carteira

### Comandos Básicos de Carteira
```bash
# Verificar endereço ativo
sui client active-address

# Verificar saldo
sui client balance

# Verificar gás disponível
sui client gas

# Solicitar tokens de teste (apenas DevNet/TestNet)
sui client faucet
```

## 🌐 Ambientes de Rede

### Configuração de Ambientes
```bash
# Listar todos os ambientes
sui client envs

# Criar ambiente personalizado
sui client new-env --alias <nome> --rpc <url-rpc>

# Alternar entre ambientes
sui client switch --env <nome-ambiente>
```

## 🏗️ Desenvolvimento Move

### Criar Projeto Move
```bash
# Criar novo projeto
sui move new <nome-do-projeto>

# Estrutura básica criada:
# ├── Move.toml
# ├── sources/
# └── tests/
```

## 📖 Guias Detalhados

- [📘 Guia Completo de Comandos](./docs/comandos-completos.md)
- [🔨 Tutorial de Desenvolvimento](./docs/tutorial-desenvolvimento.md)
- [❗ Solução de Problemas](./docs/troubleshooting.md)
- [📚 Recursos Adicionais](./docs/recursos.md)

---

## 🎯 Para Iniciantes

Se você é novo na SUI, recomendamos seguir esta ordem:

1. ✅ **Configuração**: Configure seu ambiente DevNet
2. 💰 **Carteira**: Obtenha tokens de teste com o faucet
3. 🏗️ **Primeiro Projeto**: Crie seu primeiro projeto Move
4. 📖 **Estudo**: Explore os guias detalhados

---

**💡 Dica**: Sempre use o ambiente DevNet para testes e desenvolvimento!

**Suporte**: Em caso de dúvidas, consulte a [documentação oficial da SUI](https://docs.sui.io/)