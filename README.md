


Lista 2
# INÍCIO DA QUESTÃO 6
SALARIO_MINIMO = 1621.00


class SalarioInvalidoError(Exception):
    pass


class Funcionario:
    def __init__(self, nome, salario):
        self.nome = nome
        self.salario = salario

    @property
    def salario(self):
        return self._salario

    @salario.setter
    def salario(self, valor):
        if valor < SALARIO_MINIMO:
            raise SalarioInvalidoError(
                f"Salário inválido. O mínimo é R$ {SALARIO_MINIMO:.2f}."
            )
        self._salario = valor

    def aumentar(self, percentual):
        if percentual <= 0 or percentual > 30:
            raise ValueError(
                "O percentual deve ser maior que 0 e menor ou igual a 30."
            )

        self._salario += self._salario * (percentual / 100)

# FIM DA QUESTÃO 6

# INÍCIO DA QUESTÃO 7
class EmailInvalidoError(Exception):
    pass


class Email:
    def __init__(self, endereco):
        self.endereco = endereco

    @property
    def endereco(self):
        return self._endereco

    @endereco.setter
    def endereco(self, valor):
        if "@" not in valor or "." not in valor:
            raise EmailInvalidoError("E-mail inválido.")
        self._endereco = valor

# FIM DA QUESTÃO 7

# INÍCIO DA QUESTÃO 8
def testes_questao_8():
    print("\n--- TESTES DA QUESTÃO 8 ---")

    # Caso de erro 1: salário abaixo do mínimo
    try:
        funcionario = Funcionario("Maria", 1000)
    except SalarioInvalidoError as erro:
        print("Erro de salário:", erro)

    # Caso de erro 2: percentual inválido
    try:
        funcionario = Funcionario("Maria", SALARIO_MINIMO)
        funcionario.aumentar(40)
    except ValueError as erro:
        print("Erro de aumento:", erro)

    # Caso de erro 1: e-mail sem @
    try:
        email = Email("mariaemail.com")
    except EmailInvalidoError as erro:
        print("Erro de e-mail:", erro)

    # Caso de erro 2: e-mail sem .
    try:
        email = Email("maria@email")
    except EmailInvalidoError as erro:
        print("Erro de e-mail:", erro)

# FIM DA QUESTÃO 8

# INÍCIO DA QUESTÃO 9
class ErroDeConta(Exception):
    pass


class ValorInvalidoError(ErroDeConta):
    pass


class SaldoInsuficienteError(ErroDeConta):
    pass


class LimiteExcedidoError(ErroDeConta):
    pass


class ContaBancaria:
    LIMITE_SAQUE = 1000.00

    def __init__(self, saldo=0):
        if saldo < 0:
            raise ValorInvalidoError("O saldo inicial não pode ser negativo.")
        self._saldo = saldo

    @property
    def saldo(self):
        return self._saldo

    def depositar(self, valor):
        if valor <= 0:
            raise ValorInvalidoError(
                "O valor do depósito deve ser maior que zero."
            )

        self._saldo += valor

    def sacar(self, valor):
        if valor <= 0:
            raise ValorInvalidoError(
                "O valor do saque deve ser maior que zero."
            )

        if valor > self.LIMITE_SAQUE:
            raise LimiteExcedidoError(
                "O saque não pode ultrapassar R$ 1.000,00 por operação."
            )

        if valor > self._saldo:
            raise SaldoInsuficienteError("Saldo insuficiente.")

        self._saldo -= valor

# FIM DA QUESTÃO9

# INÍCIO DA QUESTÃO 10
def caixa_eletronico():
    conta = ContaBancaria()

    while True:
        print("\n========== CAIXA ELETRÔNICO ==========")
        print("1 - Depositar")
        print("2 - Sacar")
        print("3 - Saldo")
        print("4 - Sair")

        try:
            opcao = input("Escolha uma opção: ")

            if opcao == "1":
                valor = float(input("Digite o valor do depósito: "))
                conta.depositar(valor)
                print("Depósito realizado com sucesso.")

            elif opcao == "2":
                valor = float(input("Digite o valor do saque: "))
                conta.sacar(valor)
                print("Saque realizado com sucesso.")

            elif opcao == "3":
                print(f"Saldo atual: R$ {conta.saldo:.2f}")

            elif opcao == "4":
                print("Programa encerrado.")
                break

            else:
                print("Opção inválida.")

        except ErroDeConta as erro:
            print("Erro:", erro)

        except (ValueError, TypeError):
            print("Entrada inválida. Digite um valor numérico válido.")

        except Exception as erro:
            print("Ocorreu um erro inesperado:", erro)

# FIM DA QUESTÃO 10
