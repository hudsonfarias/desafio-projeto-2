
# 📚 Desafio de Projeto - V2 | DIO 
## Otimizando o Sistema Bancário com Funções Python

Repositório criado para armazenar minhas entregas do Desafio de Projeto - Otimizando o Sistema Bancário com Funções Python do curso Dominando Python para Ciência de Dados.

Link para especificações do curso [Digital Innovation One](https://web.dio.me/track/potencia-tech-powered-ifood-ciencias-de-dados-com-python).

## 🌐 Objetivo Geral
Otimizar o sistema bancário inicial da versão 1 [Desafio de Projeto - Criando um Sistema Bancário com Python](https://github.com/hudsonfarias/desafio-projeto) criando uma versão 2 através de funções Python. Separando as funções existentes de saque, depósito e extrato em funções. Com implementação de duas novas funções, cadastrar usuário e cadastrar conta bancária.

## 📚 Regras do Desafio 

### 1ª Regra: Novas Funções
- Criar usuários, ou seja, cadastrar um cliente no banco
- Criar conta corrente, vinculando com o usuário
### 2ª Regra: Separação em Funções
- Criar funções para todas as operações do sistema bancário, a fim de deixar o código mais modulado.
### 3ª Regra: Do Saque
- O sistema deve permitir realizar somente 3 saques com limite máximo de R$ 500  por saque.
- Caso usuário não tenho saldo em conta, deverá exibir uma mensagem na tela informando que não é possível sacar o dinheiro por falta de saldo.
- Todos os saques devem ser armazenados em uma váriavel e exibidos na operação de extrato.
- A função deve receber os argumentos apenas por nomes(keyword only).
### 4ª Regra: Do Depósito 
- Aceitar somente valor de depósito positivo.
- Todos os depósitos devem ser armazenados em uma variável e exibidos na operação extrato.
- A função deve receber os argumentos apenas por posição (positional only).
### 5ª Regra: Do Extrato 
- Essa operação deve listar todos os depósitos e saque realizados na conta. 
- Ao final do extrato deverá exibir o saldo atual da conta bancária.
- Se o extrato estiver em branco, exibir uma mensagem informando que não houve movimentações na conta.
- Os valores devem ser exibidos no formato: R$ xxx.xx
- A função deve receber os argumentos por posição e nome (positional only e keyword only).
### 6ª Regra: Criar Usuário
- O programa deve armazenar os usuários em uma lista.
- Composição do cadastro de usuário: nome, data de nascimento, CPF e endereço.
- Formato da string de endereço: logradouro, nº - bairro - cidade/sigla estado.
- Formato do CPF: somente números.
- Não pode ser cadastrado mais de um usuário por CPF.
### 7ª Regra: Criar conta
- O programa deve armazenar as contas em uma lista.
- Composição da conta: agência, número da conta e usuário.
- Número de conta de forma sequencial, iniciando em 1.
- Número da agência é fixo: (0001).
- Usuário pode ter mais de uma conta, desde que seja com o mesmo CPF.

## 💻 Meu código entregue

```
import textwrap


def menu():
    menu = """\n
    ================ MENU ================
    [1]\tDepositar
    [2]\tSacar
    [3]\tExtrato
    [4]\tNova conta
    [5]\tListar contas
    [6]\tNovo usuário
    [0]\tSair
    => Informe a opção desejada: """
    return input(textwrap.dedent(menu))


def depositar(saldo, valor, extrato, /):
    if valor > 0:
        saldo += valor
        extrato += f"Depósito:\tR$ {valor:.2f}\n"
        print("\n=== Depósito realizado com sucesso! ===")
    else:
        print("\n@@@ Operação falhou! O valor informado é inválido. @@@")

    return saldo, extrato


def sacar(*, saldo, valor, extrato, limite, numero_saques, limite_saques):
    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saques = numero_saques >= limite_saques

    if excedeu_saldo:
        print("\n@@@ Operação falhou! Você não tem saldo suficiente. @@@")

    elif excedeu_limite:
        print("\n@@@ Operação falhou! O valor do saque excede o limite. @@@")

    elif excedeu_saques:
        print("\n@@@ Operação falhou! Número máximo de saques excedido. @@@")

    elif valor > 0:
        saldo -= valor
        extrato += f"Saque:\t\tR$ {valor:.2f}\n"
        numero_saques += 1
        print("\n=== Saque realizado com sucesso! ===")

    else:
        print("\n@@@ Operação falhou! O valor informado é inválido. @@@")

    return saldo, extrato


def exibir_extrato(saldo, /, *, extrato):
    print("\n================ EXTRATO ================")
    print("Não foram realizadas movimentações." if not extrato else extrato)
    print(f"\nSaldo:\t\tR$ {saldo:.2f}")
    print("==========================================")


def criar_usuario(usuarios):
    cpf = input("Informe o CPF (somente número): ")
    usuario = filtrar_usuario(cpf, usuarios)

    if usuario:
        print("\n@@@ Já existe usuário com esse CPF! @@@")
        return

    nome = input("Informe o nome completo: ")
    data_nascimento = input("Informe a data de nascimento (dd-mm-aaaa): ")
    endereco = input("Informe o endereço (logradouro, nro - bairro - cidade/sigla estado): ")

    usuarios.append({"nome": nome, "data_nascimento": data_nascimento, "cpf": cpf, "endereco": endereco})

    print("=== Usuário criado com sucesso! ===")


def filtrar_usuario(cpf, usuarios):
    usuarios_filtrados = [usuario for usuario in usuarios if usuario["cpf"] == cpf]
    return usuarios_filtrados[0] if usuarios_filtrados else None


def criar_conta(agencia, numero_conta, usuarios):
    cpf = input("Informe o CPF do usuário: ")
    usuario = filtrar_usuario(cpf, usuarios)

    if usuario:
        print("\n=== Conta criada com sucesso! ===")
        return {"agencia": agencia, "numero_conta": numero_conta, "usuario": usuario}

    print("\n@@@ Usuário não encontrado, fluxo de criação de conta encerrado! @@@")


def listar_contas(contas):
    for conta in contas:
        linha = f"""\
            Agência:\t{conta['agencia']}
            C/C:\t\t{conta['numero_conta']}
            Titular:\t{conta['usuario']['nome']}
        """
        print("=" * 100)
        print(textwrap.dedent(linha))


def main():
    LIMITE_SAQUES = 3
    AGENCIA = "0001"

    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0
    usuarios = []
    contas = []

    while True:
        opcao = menu()

        if opcao == "1":
            valor = float(input("Informe o valor do depósito: "))

            saldo, extrato = depositar(saldo, valor, extrato)

        elif opcao == "2":
            valor = float(input("Informe o valor do saque: "))

            saldo, extrato = sacar(
                saldo=saldo,
                valor=valor,
                extrato=extrato,
                limite=limite,
                numero_saques=numero_saques,
                limite_saques=LIMITE_SAQUES,
            )

        elif opcao == "3":
            exibir_extrato(saldo, extrato=extrato)

        elif opcao == "6":
            criar_usuario(usuarios)

        elif opcao == "4":
            numero_conta = len(contas) + 1
            conta = criar_conta(AGENCIA, numero_conta, usuarios)

            if conta:
                contas.append(conta)

        elif opcao == "5":
            listar_contas(contas)

        elif opcao == "0":
            break

        else:
            print("Operação inválida, por favor selecione novamente a operação desejada.")


main()

```
## 💡 Pontos Importantes
- Utilizei o import textwarp para formatar o texto da melhor maneira possível.
- Criei uma função de filtro para vincular um usuário a uma conta buscando pelo número de CPF informado para cada usuário na lista.

## 📸Screenshots
### Menu
![image](https://github.com/hudsonfarias/desafio-projeto/assets/138171591/d89e9e8b-b227-43e2-8eb8-ae86618b34a0)
### Depósito
![image](https://github.com/hudsonfarias/desafio-projeto/assets/138171591/ea782349-1e3d-459e-8847-7cfa30fe6b60)
### Saque
![image](https://github.com/hudsonfarias/desafio-projeto/assets/138171591/f20ac5e1-48ff-477a-a04d-8920c11a8aa4)
### Extrato 
![image](https://github.com/hudsonfarias/desafio-projeto/assets/138171591/d92aeb08-e816-43ba-b214-69c2630fb473)
### Criando Usuário
![image](https://github.com/hudsonfarias/desafio-projeto/assets/138171591/0cfcf564-e912-440d-9853-3be3cd9d7f87)
### Criando Conta
![image](https://github.com/hudsonfarias/desafio-projeto/assets/138171591/bcbd5d92-e104-4f0b-a4af-3ae2981f1a6b)
### Listando Contas
![image](https://github.com/hudsonfarias/desafio-projeto/assets/138171591/87a0a272-7901-49c9-aeb9-1e0812e1127a)


##### Concluindo mais uma parte do meu aprendizado, eu pude agregar a meus conhecimentos assuntos de Python sobre listas, tuplas, conjuntos, dicionários e funções.

Obrigado por ver meu projeto! 😀
