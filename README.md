from __future__ import annotations
from datetime import date


class QuantidadeInvalidaError(Exception):
    pass


class MedicamentoVencidoError(Exception):
    pass


class Medicamento:

    def __init__(
        self,
        nome: str,
        lote: str,
        validade: date,
        quantidade: int,
        valor: float
    ):
        self.nome = nome
        self.lote = lote
        self.validade = validade
        self.quantidade = quantidade
        self.valor = valor

    @property
    def quantidade(self):
        return self._quantidade

    @quantidade.setter
    def quantidade(self, valor):
        if valor < 0:
            raise ValueError("Quantidade não pode ser negativa!")

        self._quantidade = valor

    @property
    def valor(self):
        return self._valor

    @valor.setter
    def valor(self, valor):
        if valor <= 0:
            raise ValueError("Valor deve ser maior que zero!")

        self._valor = valor

    @classmethod
    def de_registro(cls, registro):
        nome, lote, validade, quantidade, valor = registro.split(";")

        return cls(
            nome,
            lote,
            date.fromisoformat(validade),
            int(quantidade),
            float(valor)
        )

    @staticmethod
    def dias_para_vencer(validade):
        return (validade - date.today()).days

    def dispensar(self, quantidade):
        if quantidade <= 0:
            raise QuantidadeInvalidaError(
                "A quantidade deve ser maior que zero."
            )

        if quantidade > self.quantidade:
            raise QuantidadeInvalidaError(
                "Quantidade solicitada maior que o estoque."
            )

        if self.validade < date.today():
            raise MedicamentoVencidoError(
                "Não é possível dispensar medicamento vencido."
            )

        self.quantidade -= quantidade

    def repor(self, quantidade: int):
        self.quantidade += quantidade

    def __str__(self):
        return (
            f"{self.nome} - {self.quantidade} unidades - "
            f"Validade: {self.validade}"
        )

    def __repr__(self):
        return (
            f"Medicamento("
            f"{self.nome}, {self.lote}, "
            f"{self.validade}, {self.quantidade}, {self.valor})"
        )

    def __eq__(self, outro):
        return (
            self.nome == outro.nome
            and self.lote == outro.lote
        )

    def __lt__(self, outro):
        return self.validade < outro.validade


if __name__ == "__main__":

    medicamento = Medicamento(
        "Paracetamol",
        "L001",
        date(2026, 12, 31),
        100,
        10.50
    )

    print(medicamento)

    medicamento.dispensar(20)
    print("Quantidade:", medicamento.quantidade)

    medicamento.repor(50)
    print("Quantidade:", medicamento.quantidade)

    print(
        "Dias para vencer:",
        Medicamento.dias_para_vencer(medicamento.validade)
    )

    registro = "Dipirona;L002;2027-05-20;200;8.50"

    medicamento2 = Medicamento.de_registro(registro)

    print(medicamento2)
