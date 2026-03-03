# 📌 Sistema Bancário Orientado a Objetos em Python

Este código implementa um sistema bancário simples via terminal, utilizando Programação Orientada a Objetos (POO).

**Ele permite:**
- Criar clientes
- Criar contas correntes
- Realizar depósitos
- Realizar saques
- Emitir extrato
- Listar contas

---

## 🏗 Estrutura Geral do Sistema

O sistema é composto por:

**Entidades principais**
- `Cliente`
- `PessoaFisica`
- `Conta`
- `ContaCorrente`

**Histórico**
- Camada de transações
- `Transacao` (classe abstrata)
- `Saque`
- `Deposito`

**Camada de interface**
- Funções de menu e operações (`depositar`, `sacar`, etc.)
- `main()` que controla o fluxo do programa

---

## 👤 Classe `Cliente`

Representa um cliente genérico do banco.

```python
class Cliente:
```

**Atributos:**
- `endereco`
- `contas` (lista de contas associadas)

**Métodos:**
- `realizar_transacao(conta, transacao)`
- `adicionar_conta(conta)`

> Essa classe serve como classe base.

---

## 👨 Classe `PessoaFisica`

Herda de `Cliente`.

```python
class PessoaFisica(Cliente):
```

**Atributos adicionais:**
- `nome`
- `data_nascimento`
- `cpf`

> Representa um cliente do tipo pessoa física.

---

## 🏦 Classe `Conta`

Representa uma conta bancária genérica.

```python
class Conta:
```

**Atributos protegidos:**
- `_saldo`
- `_numero`
- `_agencia`
- `_cliente`
- `_historico`

**Propriedades:**
- `saldo`, `numero`, `agencia`, `cliente`, `historico`

**Métodos principais:**
- `sacar(valor)`
- `depositar(valor)`

**Regras:**
- Não permite saque maior que o saldo
- Não permite valores negativos
- Retorna `True` ou `False` indicando sucesso

---

## 💳 Classe `ContaCorrente`

Especialização de `Conta`.

```python
class ContaCorrente(Conta):
```

**Regras adicionais:**
- Limite de valor por saque (`_limite = 500`)
- Limite de número de saques (`_limite_saques = 3`)

> Sobrescreve o método `sacar()` para aplicar essas validações extras.

---

## 📜 Classe `Historico`

Responsável por registrar as transações realizadas.

```python
class Historico:
```

Armazena uma lista de dicionários contendo:

```python
{
    "tipo": "Saque" ou "Deposito",
    "valor": valor,
    "data": timestamp
}
```

> Sempre que uma transação é concluída com sucesso, ela é registrada aqui.

---

## 🔄 Classe Abstrata `Transacao`

Define o contrato para qualquer tipo de transação.

```python
class Transacao(ABC):
```

**Métodos abstratos:**
- `valor`
- `registrar(conta)`

> Isso obriga subclasses a implementarem esses métodos.

---

## 💰 Classe `Deposito`

```python
class Deposito(Transacao):
```

- Executa `conta.depositar(valor)`
- Se bem-sucedido → registra no histórico

---

## 💸 Classe `Saque`

```python
class Saque(Transacao):
```

- Executa `conta.sacar(valor)`
- Se bem-sucedido → registra no histórico

---

## 🖥 Interface do Sistema

### Função `menu()`

Exibe o menu interativo:

```
[d] Depositar
[s] Sacar
[e] Extrato
[nc] Nova conta
[lc] Listar contas
[nu] Novo usuário
[q] Sair
```

---

## 🔍 Funções Operacionais

### `filtrar_cliente(cpf, clientes)`
Busca cliente pelo CPF.

### `recuperar_conta_cliente(cliente)`
Retorna a primeira conta do cliente.

> ⚠️ **Observação:** Há um comentário indicando melhoria futura:
> ```python
> # FIXME: não permite cliente escolher a conta
> ```
> Atualmente o sistema sempre usa a primeira conta da lista.

### `depositar(clientes)`

**Fluxo:**
1. Solicita CPF
2. Busca cliente
3. Solicita valor
4. Executa transação

### `sacar(clientes)`

Fluxo idêntico ao depósito, mas usando `Saque`.

### `exibir_extrato(clientes)`

Exibe:
- Lista de transações
- Saldo atual

### `criar_cliente(clientes)`

- Cria nova instância de `PessoaFisica`
- Valida duplicidade de CPF

### `criar_conta(numero_conta, clientes, contas)`

Cria nova `ContaCorrente` associada ao cliente.

### `listar_contas(contas)`

Imprime todas as contas cadastradas.

---

## 🔁 Função `main()`

Controla o loop principal do sistema:

```python
while True:
    ...
```

Executa continuamente até o usuário escolher `q` (sair).

---

## 📌 Conceitos de POO Aplicados

| Conceito | Onde é usado |
|---|---|
| **Encapsulamento** | Uso de `_atributos` e `@property` |
| **Herança** | `PessoaFisica → Cliente` |
| **Polimorfismo** | `ContaCorrente.sacar()` |
| **Abstração** | Classe abstrata `Transacao` |
| **Composição** | `Conta` contém `Historico` |