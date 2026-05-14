
print("Bem-vindo à Loja Online")

class Cliente:
    def __init__(self, usuario, nome, sobrenome, idade, email, senha, cpf):
        self.usuario = usuario
        self.nome = nome
        self.sobrenome = sobrenome
        self.idade = idade
        self.email = email
        self.senha = senha
        self.cpf = cpf


lista_clientes = []
log_sistema = []
historico_compras = []

cupons = {
    "LOJA10": 10,
    "PROMO20": 20,
    "CLIENTE5": 5
}

produtos = {
    1: {"nome": "Camiseta", "valor": 49.90, "quantidade": 10},
    2: {"nome": "Calça Jeans", "valor": 119.90, "quantidade": 10},
    3: {"nome": "Tênis", "valor": 249.90, "quantidade": 10},
    4: {"nome": "Boné", "valor": 39.90, "quantidade": 10},
    5: {"nome": "Jaqueta", "valor": 199.90, "quantidade": 10},
    6: {"nome": "Relógio", "valor": 159.90, "quantidade": 10},
    7: {"nome": "Mochila", "valor": 89.90, "quantidade": 10},
    8: {"nome": "Óculos", "valor": 129.90, "quantidade": 10},
    9: {"nome": "Perfume", "valor": 179.90, "quantidade": 10}
}


def menu_principal():
    print(" MENU PRINCIPAL ")
    print("1 - Cadastrar Cliente")
    print("2 - Login")
    print("3 - Sair")


def menu_loja():
    print(" LOJA ONLINE ")
    print("1 - Comprar Produto")
    print("2 - Cadastrar Produto")
    print("3 - Listar Produtos")
    print("4 - Repor Estoque")
    print("5 - Relatório (LOG)")
    print("6 - Voltar")
    print("7 - Sair")
    print("8 - Ver Cupons")
    print("9 - Histórico de Compras")


while True:

    menu_principal()

    opcao = int(input("Escolha uma opção: "))

    if opcao == 1:

        continuar = "s"

        while continuar == "s":

            usuario = input("Digite seu usuário: ")
            nome = input("Digite seu nome: ")
            sobrenome = input("Digite seu sobrenome: ")
            idade = int(input("Digite sua idade: "))
            email = input("Digite seu e-mail: ")
            senha = input("Digite sua senha: ")
            cpf = input("Digite seu CPF: ")

            cliente = Cliente(
                usuario,
                nome,
                sobrenome,
                idade,
                email,
                senha,
                cpf
            )

            lista_clientes.append(cliente)

            log_sistema.append(
                f"Cliente {usuario} cadastrado com sucesso"
            )

            print("Cadastro realizado com sucesso!")

            continuar = input(
                "Deseja cadastrar outro cliente? (s/n): "
            ).lower()

    elif opcao == 2:

        tentativas = 0
        login_sucesso = False

        while tentativas < 3:

            usuario_login = input("Digite seu usuário: ")
            senha_login = input("Digite sua senha: ")

            for cliente in lista_clientes:

                if (
                    cliente.usuario == usuario_login
                    and cliente.senha == senha_login
                ):

                    login_sucesso = True
                    break

            if login_sucesso:

                print("Login realizado com sucesso!")

                log_sistema.append(
                    f"Cliente {usuario_login} fez login"
                )

                break

            else:

                tentativas += 1

                print(
                    f"Usuário ou senha incorretos! "
                    f"Tentativas restantes: {3 - tentativas}"
                )

        if not login_sucesso:
            print("Sistema bloqueado por excesso de tentativas!")
            continue

        while True:

            menu_loja()

            escolha = int(input("Escolha uma opção: "))

            if escolha == 1:

                print(" PRODUTOS DISPONÍVEIS                           ")

                for codigo, produto in produtos.items():

                    print(
                        f"{codigo} - "
                        f"{produto['nome']} | "
                        f"R$ {produto['valor']:.2f} | "
                        f"Estoque: {produto['quantidade']}"
                    )

                print("0 - Voltar")

                codigo_produto = int(
                    input("Digite o código do produto: ")
                )

                if codigo_produto == 0:
                    continue

                if codigo_produto not in produtos:
                    print("Código inválido!")
                    continue

                quantidade = int(
                    input("Digite a quantidade desejada: ")
                )

                if quantidade <= 0:
                    print("Quantidade inválida!")
                    continue

                produto = produtos[codigo_produto]

                if quantidade > produto["quantidade"]:
                    print("Estoque insuficiente!")
                    continue

                produto["quantidade"] -= quantidade

                total = produto["valor"] * quantidade

                print(f"\nProduto: {produto['nome']}")
                print(f"Quantidade: {quantidade}")
                print(f"Valor sem desconto: R$ {total:.2f}")

                usar_cupom = input(
                    "Deseja usar cupom? (s/n): "
                ).lower()

                desconto = 0

                if usar_cupom == "s":

                    codigo_cupom = input(
                        "Digite o cupom: "
                    ).upper()

                    if codigo_cupom in cupons:

                        desconto = cupons[codigo_cupom]

                        valor_desconto = total * (desconto / 100)

                        total -= valor_desconto

                        print(
                            f"Cupom aplicado: "
                            f"{desconto}% OFF"
                        )

                    else:
                        print("Cupom inválido!")

                print(f"Valor final: R$ {total:.2f}")

                print(" PAGAMENTO ")
                print("1 - Pix")
                print("2 - Cartão")
                print("3 - Dinheiro")

                pagamento = int(
                    input("Escolha a forma de pagamento: ")
                )

                if pagamento == 1:
                    forma_pagamento = "Pix"

                elif pagamento == 2:
                    forma_pagamento = "Cartão"

                elif pagamento == 3:
                    forma_pagamento = "Dinheiro"

                else:
                    print("Forma de pagamento inválida!")
                    continue

                print(
                    f"Pagamento realizado via "
                    f"{forma_pagamento}"
                )

                historico_compras.append(
                    f"Produto: {produto['nome']} | "
                    f"Quantidade: {quantidade} | "
                    f"Pagamento: {forma_pagamento} | "
                    f"Valor pago: R$ {total:.2f}"
                )

                print("Compra realizada com sucesso!")

                log_sistema.append(
                    f"Compra realizada: "
                    f"{produto['nome']} | "
                    f"Quantidade: {quantidade} | "
                    f"Pagamento: {forma_pagamento} | "
                    f"Total: R$ {total:.2f}"
                )

            elif escolha == 2:

                print("  CADASTRAR PRODUTO ")

                nome_produto = input("Nome do produto: ")
                valor_produto = float(
                    input("Valor do produto: R$ ")
                )
                quantidade_produto = int(
                    input("Quantidade em estoque: ")
                )

                novo_codigo = max(produtos.keys()) + 1

                produtos[novo_codigo] = {
                    "nome": nome_produto,
                    "valor": valor_produto,
                    "quantidade": quantidade_produto
                }

                print("Produto cadastrado com sucesso!")

                log_sistema.append(
                    f"Produto cadastrado: "
                    f"{nome_produto}"
                )

            elif escolha == 3:

                print(" LISTA DE PRODUTOS ")

                for codigo, produto in produtos.items():

                    print(
                        f"{codigo} - "
                        f"{produto['nome']} | "
                        f"R$ {produto['valor']:.2f} | "
                        f"Estoque: {produto['quantidade']}"
                    )

            elif escolha == 4:

                print(" REPOSIÇÃO DE ESTOQUE ")

                for codigo, produto in produtos.items():

                    print(
                        f"{codigo} - "
                        f"{produto['nome']} | "
                        f"Estoque atual: "
                        f"{produto['quantidade']}"
                    )

                reposicao = int(
                    input("Digite o código do produto: ")
                )

                if reposicao not in produtos:
                    print("Produto inválido!")
                    continue

                quantidade = int(
                    input("Quantidade para adicionar: ")
                )

                if quantidade <= 0:
                    print("Quantidade inválida!")
                    continue

                produtos[reposicao]["quantidade"] += quantidade

                produto = produtos[reposicao]["nome"]

                print(
                    f"Estoque do produto "
                    f"{produto} atualizado!"
                )

                log_sistema.append(
                    f"Reposição feita no produto "
                    f"{produto}"
                )

            elif escolha == 5:

                print(" RELATÓRIO LOG ")

                if len(log_sistema) == 0:
                    print("Nenhuma movimentação registrada.")

                else:

                    for log in log_sistema:
                        print(log)

            elif escolha == 6:
                break

            elif escolha == 7:

                print("Volte sempre!")
                exit()

            elif escolha == 8:

                print(" CUPONS DISPONÍVEIS ")
                print("LOJA10 = 10% OFF")
                print("PROMO20 = 20% OFF")
                print("CLIENTE5 = 5% OFF")

            elif escolha == 9:

                print("\n HISTÓRICO DE COMPRAS ")

                if len(historico_compras) == 0:
                    print("Nenhuma compra realizada.")

                else:

                    for compra in historico_compras:
                        print(compra)

            else:
                print("Opção inválida!")

    elif opcao == 3:

        print("Volte sempre!")
        break

    else:
        print("Opção inválida!")
