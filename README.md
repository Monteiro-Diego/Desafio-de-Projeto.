import textwrap

def menu():
    menu = """
    ========== MENU =========

    [d] Depositar
    [s] Sacar
    [e] Extrato
    [nc] Nova conta
    [lc] Listar contas
    [nu] Novo usuario
    [q] Sair

    => """
    return input(textwrap.dedent(menu))

def depositar(saldo,valor,extrato,/):
    if valor > 0:
        saldo += valor
        extrato += f"Depósito:\tR$ {valor:.2f}\n"
        print("\n ==Depósito realizado com sucesso! ==")
    else:
        print("\n  Operação falhou! Valor informado não é valido.")
    return saldo,extrato


def sacar(*, saldo, valor, extrato, limite, numero_saques, limite_saques):

    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saques = numero_saques >= limite_saques

    if excedeu_saldo:
        print("Erro! Você não possui saldo suficiente")

    elif excedeu_limite:
        print("Erro! O valor do saque excedeu o limite")

    elif excedeu_saques:
        print("Erro! Numero máximo de saques excedido")

    elif valor > 0:
        saldo -= valor
        extrato += f"Saque:\tR$ {valor:.2f}\n"
    else:
        print("Operação Falhou, o valor informado é invalido")


def exibir_extrato(saldo, /, *, extrato):
    print("===== extrato =====")
    print("Não foram realizadas movimentações." if not extrato else extrato)
    print(f"\nSaldo:\t\tR$ {saldo:.2.f}")
    print("-------------------------")

def criar_usuario(usuarios):
    cpf = input("informe o CPF (Somente número):")
    usuario = filtrar_usuario(cpf,usuarios)

    if usuario:
        print("\n Erro! Já existe usuário com esse CPF!")

    nome = input("Informe o nome completo: ")
    data_nascimento = input("Informe a adata de nascimento (dd-mm-aaaa): ")
    endereco = input("informe o endereço (logradouro, nro - bairro - cidade/sigla estado):")
    usuarios.append({"nome":nome, "data_nascimento":data_nascimento, "cpf":cpf, "endereco":endereco})

    print("== Usuario criado com sucesso! ==")

def filtrar_usuario(cpf, usuarios):
    usuarios_filtrados = [usuario for usuario in usuarios if usuario["cpf"] ==cpf]
    return usuarios_filtrados[0] if usuarios_filtrados else None

def criar_conta(agencia, numero_conta, usuarios):
    cpf = input("informe o CPF do usuário:")
    usuario = filtrar_usuario(cpf,usuarios)

    if usuario:
        print("\n === Conta criada com sucesso! ===")
        return {"agencia":agencia, "numero_conta":numero_conta, "usuario":usuario}

    print("\n Erro! Usuário não encontrado, fluxo de criação de conta encerrado! ")
    return None

def listar_contas(contas):
    for conta in contas:
        linha = f"""\
            Agência: \t{conta['agencia']}
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

        if opcao == "d":
            valor = float(input("informe o valor do depósito:"))

            saldo, extrato = depositar(saldo, valor, extrato)

        elif opcao == "s":
            valor = float(input("informe o valor do saque:"))

            saldo, extrato = sacar(
                saldo=saldo,
                valor=valor,
                extrato=extrato,
                limite=limite,
                numero_saques=numero_saques,
                limite_saques=LIMITE_SAQUES,
            )

        elif opcao == "e":
            exibir_extrato(saldo, extrato=extrato)

        elif opcao == "nu":
            criar_usuario(usuarios)

        elif opcao == "nc":
            numero_conta=len(contas +1)
            conta = criar_conta(AGENCIA,numero_conta,usuarios)

        elif opcao == "lc":
            listar_contas(contas)

        elif opcao == "q":
            break
        else:
            print("Operação inválida, por favor selecione novamente a operção desejada.")
