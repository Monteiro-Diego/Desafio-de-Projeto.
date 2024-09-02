#Sistema bancario de entrada, saida e extrato do dinheiro em Python

menu = """

[s] Sacar

[d] Depositar

[e] Extrato

[q] Sair 



=> """



saldo = 0

limite = 500

extrato = ""

numero_saque = 0

limite_saques = 3



while True:



  opcao = input(menu)



  if opcao == "d":

    print("\n == Depositar ==")

    valor = float(input("Informe o valor do depósito"))

    if valor > 0:

      saldo += valor

      extrato += f"Deposito: R$ {valor:.2f}\n"

    else: 

      print("operação falhou! valor informado é invalido.")



  elif opcao == "s":

    print("\n Valor disponivel para saque:",saldo)

    valor = float(input("Informe o valor do saque:"))

     

    excedeu_saldo = valor > saldo

    excedeu_limite = valor > limite

    excedeu_saque = numero_saque >= limite_saques

     

    if excedeu_saldo:

      print("Operação falhou, voce não tem saldo suficiente!")

   

    elif excedeu_limite:

      print("Operação falhou, o valor do saldo excedeu o limite de 500 reais!")

   

    elif excedeu_saque:

      print("Operação falhou, voce já realizou numero maximo de saques")

     

    elif valor > 0:

      saldo -= valor

      extrato += f"saque: R$ {valor:.2f}\n"

      numero_saque += 1

   

  elif opcao == "e":

    print("\n ========= Extrato =========")

    print("Não foi realizado movimentações." if not extrato else extrato)

    print(extrato)

    print("Saldo atual",saldo)

   

  elif opcao == "q":

    print("Sair")

    break

   

  else:

    print("Operação invalida, por favor informar uma das operações acima")
