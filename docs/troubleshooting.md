# ❗ Solução de Problemas SUI

## 🔧 Problemas Comuns e Soluções

### 1. 🌐 Problemas de Conexão

#### Erro: "Failed to connect to RPC"
**Sintomas:**
```
Error: Failed to connect to RPC server
```

**Soluções:**
```bash
# 1. Verificar ambiente ativo
sui client active-env

# 2. Verificar configuração de ambientes
sui client envs

# 3. Recriar ambiente DevNet
sui client new-env --alias devnet --rpc https://fullnode.devnet.sui.io:443

# 4. Alternar para o ambiente correto
sui client switch --env devnet
```

#### Erro: "Network timeout"
**Soluções:**
```bash
# 1. Verificar conexão com internet
ping google.com

# 2. Tentar ambiente alternativo
sui client new-env --alias devnet2 --rpc https://sui-devnet.public.blastapi.io

# 3. Verificar firewall/proxy
```

### 2. 💰 Problemas com Faucet

#### Erro: "Faucet request failed"
**Sintomas:**
```
Error: Faucet request failed: Too many requests
```

**Soluções:**
```bash
# 1. Aguardar cooldown (geralmente 24h)
# 2. Usar faucet web alternativo:
# https://discord.gg/sui (canal #devnet-faucet)

# 3. Verificar se está no ambiente correto
sui client active-env

# 4. Verificar endereço
sui client active-address
```

#### Saldo Zero Após Faucet
**Verificações:**
```bash
# 1. Confirmar recebimento
sui client balance

# 2. Verificar histórico de transações
sui client objects

# 3. Verificar ambiente
sui client active-env
```

### 3. 🏗️ Problemas de Compilação Move

#### Erro: "Package not found"
**Sintomas:**
```
Error: Unable to resolve package dependency
```

**Soluções:**
```bash
# 1. Verificar Move.toml
cat Move.toml

# 2. Atualizar dependências
sui move build --fetch-deps-only

# 3. Verificar versão do framework
# No Move.toml, usar:
[dependencies]
Sui = { git = "https://github.com/MystenLabs/sui.git", subdir = "crates/sui-framework/packages/sui-framework", rev = "framework/devnet" }
```

#### Erro: "Compilation failed"
**Verificações:**
```move
// 1. Verificar sintaxe Move
// 2. Verificar imports
use sui::object::{Self, UID};
use sui::transfer;
use sui::tx_context::{Self, TxContext};

// 3. Verificar estruturas
public struct MinhaStruct has key {
    id: UID,
    // outros campos...
}
```

### 4. 🚀 Problemas de Deploy

#### Erro: "Insufficient gas"
**Sintomas:**
```
Error: Insufficient gas
```

**Soluções:**
```bash
# 1. Verificar gás disponível
sui client gas

# 2. Solicitar mais tokens
sui client faucet

# 3. Aumentar budget de gás
sui client publish --gas-budget 30000000

# 4. Usar objeto de gás específico
sui client publish --gas <OBJECT_ID> --gas-budget 20000000
```

#### Erro: "Package already exists"
**Soluções:**
```bash
# 1. Incrementar versão no Move.toml
version = "0.0.2"

# 2. Ou fazer upgrade do package
sui client upgrade --package-id <PACKAGE_ID>
```

### 5. 🔑 Problemas de Chaves/Endereços

#### Erro: "Private key not found"
**Soluções:**
```bash
# 1. Verificar endereços disponíveis
sui client addresses

# 2. Gerar novo endereço
sui client new-address ed25519

# 3. Alternar endereço ativo
sui client switch --address <ENDEREÇO>

# 4. Em último caso, resetar configuração
rm -rf ~/.sui/sui_config
sui client
```

#### Problema: "Endereço mudou"
**Verificações:**
```bash
# 1. Listar todos os endereços
sui client addresses

# 2. Verificar endereço ativo
sui client active-address

# 3. Verificar ambiente ativo
sui client active-env
```

### 6. 🔄 Problemas de Ambiente

#### Ambientes Misturados
**Sintomas:** Transações não encontradas, objetos sumindo

**Soluções:**
```bash
# 1. Verificar ambiente atual
sui client active-env

# 2. Listar todos os ambientes
sui client envs

# 3. Limpar e reconfigurar
sui client new-env --alias devnet --rpc https://fullnode.devnet.sui.io:443
sui client switch --env devnet

# 4. Verificar configuração
cat ~/.sui/sui_config/client.yaml
```

### 7. 📦 Problemas de Objetos

#### Objeto Não Encontrado
**Sintomas:**
```
Error: Object not found
```

**Verificações:**
```bash
# 1. Listar objetos próprios
sui client objects

# 2. Verificar ambiente correto
sui client active-env

# 3. Verificar se objeto existe
sui client object <OBJECT_ID>
```

## 🆘 Comandos de Emergência

### Reset Completo
```bash
# ⚠️ CUIDADO: Isso apaga TODA a configuração
rm -rf ~/.sui
sui client
```

### Backup da Configuração
```bash
# Fazer backup
cp -r ~/.sui ~/.sui_backup

# Restaurar backup
cp -r ~/.sui_backup ~/.sui
```

### Verificação de Saúde
```bash
# Script de verificação completa
echo "=== Verificação SUI ==="
echo "Versão SUI:"
sui --version

echo -e "\nAmbiente Ativo:"
sui client active-env

echo -e "\nEndereço Ativo:"
sui client active-address

echo -e "\nSaldo:"
sui client balance

echo -e "\nGás Disponível:"
sui client gas

echo -e "\nAmbientes Configurados:"
sui client envs
```

## 📞 Onde Buscar Ajuda

1. **Documentação Oficial**: https://docs.sui.io/
2. **Discord SUI**: https://discord.gg/sui
3. **GitHub Issues**: https://github.com/MystenLabs/sui/issues
4. **Stack Overflow**: Tag `sui-blockchain`
5. **Reddit**: r/sui

## 💡 Dicas de Prevenção

1. **Sempre verifique o ambiente** antes de executar comandos
2. **Faça backup das configurações** regularmente
3. **Use DevNet para testes** sempre
4. **Mantenha a SUI CLI atualizada**
5. **Documente mudanças** no seu projeto

## 🔍 Logs e Debug

### Habilitar Logs Detalhados
```bash
export RUST_LOG=debug
sui client <comando>
```

### Verificar Logs do Sistema
```bash
# Linux/Mac
tail -f ~/.sui/sui_config/sui.log

# Verificar configuração
cat ~/.sui/sui_config/client.yaml
```