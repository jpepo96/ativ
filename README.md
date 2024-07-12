def exibir_menu():
    menu = """
    [d] Depositar
    [s] Sacar
    [e] Extrato
    [c] Criar Conta
    [l] Listar Contas
    [q] Sair

    => """
    return input(menu)

def criar_usuario():
    nome = input("Informe seu nome: ")
    usuario_id = input("Informe um ID de usuário: ")
    return {"nome": nome, "usuario_id": usuario_id}

def criar_conta(contas):
    conta_numero = input("Informe o número da conta: ")
    agencia = input("Informe a agência: ")
    contas.append({"conta_numero": conta_numero, "agencia": agencia})
    print("Conta criada com sucesso!")
    return contas

def listar_contas(contas):
    if contas:
        print("\n=========== LISTA DE CONTAS ===========")
        for conta in contas:
            print(f"Conta: {conta['conta_numero']} | Agência: {conta['agencia']}")
        print("=======================================")
    else:
        print("Não há contas cadastradas.")

def depositar(saldo, extrato):
    valor = float(input("Informe o valor do depósito: "))
    if valor > 0:
        saldo += valor
        extrato += f"Depósito: R$ {valor:.2f}\n"
    else:
        print("Operação falhou! O valor informado é inválido.")
    return saldo, extrato

def sacar(saldo, extrato, limite, numero_saques, LIMITE_SAQUES):
    valor = float(input("Informe o valor do saque: "))

    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saques = numero_saques >= LIMITE_SAQUES

    if excedeu_saldo:
        print("Operação falhou! Você não tem saldo suficiente.")
    elif excedeu_limite:
        print("Operação falhou! O valor do saque excede o limite.")
    elif excedeu_saques:
        print("Operação falhou! Número máximo de saques excedido.")
    elif valor > 0:
        saldo -= valor
        extrato += f"Saque: R$ {valor:.2f}\n"
        numero_saques += 1
    else:
        print("Operação falhou! O valor informado é inválido.")
    
    return saldo, extrato, numero_saques

def exibir_extrato(saldo, extrato):
    print("\n================ EXTRATO ================")
    print("Não foram realizadas movimentações." if not extrato else extrato)
    print(f"\nSaldo: R$ {saldo:.2f}")
    print("==========================================")

def main():
    usuario = criar_usuario()
    contas = []
    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0
    LIMITE_SAQUES = 3

    print(f"\nBem-vindo(a), {usuario['nome']} (ID: {usuario['usuario_id']})!\n")

    while True:
        opcao = exibir_menu()

        if opcao == "d":
            saldo, extrato = depositar(saldo, extrato)
        elif opcao == "s":
            saldo, extrato, numero_saques = sacar(saldo, extrato, limite, numero_saques, LIMITE_SAQUES)
        elif opcao == "e":
            exibir_extrato(saldo, extrato)
        elif opcao == "c":
            contas = criar_conta(contas)
        elif opcao == "l":
            listar_contas(contas)
        elif opcao == "q":
            print(f"\nObrigado por utilizar nosso sistema, {usuario['nome']}! Até a próxima.")
            break
        else:
            print("Operação inválida, por favor selecione novamente a operação desejada.")

if __name__ == "__main__":
    main()
