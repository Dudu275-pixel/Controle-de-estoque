class ArrayList:
    def __init__(self):
        self.MEMORY_SPACE = 10
        self.ultima_posicao = 0
        self.array = [None] * self.MEMORY_SPACE
        
    def get(self, posicao:int):
        if (posicao < 0 or posicao > (self.tamanho() - 1)):
            raise IndexError("Índice fora do limite")
        return self.array[posicao]
    
    def atualizar_remocao(self, inicio: int, fim: int):
        for i in range(inicio, fim):
            self.array[i] = self.array[i + 1]
        self.ultima_posicao -= 1
        
    def atualizar_insercao(self, inicio: int, fim: int):
        for i in range(inicio, fim, -1):
            self.array[i] = self.array[i-1]
        self.ultima_posicao += 1
        
    def adicionar(self, valor):
        if (self.ultima_posicao == self.capacidade()):
            self.redimensionar()
        self.array[self.ultima_posicao] = valor
        self.ultima_posicao += 1
        
    def remover(self):
        ultimo = self.array[self.ultima_posicao - 1]
        self.ultima_posicao -= 1
        return ultimo
    
    def capacidade(self):
        return len(self.array)
    
    def tamanho(self):
        return self.ultima_posicao 

    def redimensionar(self):
        print("Aumentando memória...")
        novo_array = [None] * (self.capacidade() * 2)
        for i in range(self.capacidade()):
            novo_array[i] = self.array[i]
        self.array = novo_array




class Pilha:
    def __init__(self):
        self.lista = ArrayList()
    
    def empilhar(self, valor):
        self.lista.adicionar(valor)
    
    def desempilhar(self):
        if self.vazia():
            print("Nada para desfazer")
            return None
        return self.lista.remover()
    
    def vazia(self):
        return self.lista.tamanho() == 0


# EStoque

class SistemaEstoque:
    def __init__(self):
        self.produtos = []
        self.historico = Pilha()
    
    def adicionar_produto(self, nome):
        self.produtos.append(nome)
        self.historico.empilhar(("adicionar", nome))
        print(f"Produto '{nome}' adicionado")
    
    def remover_produto(self, nome):
        if nome in self.produtos:
            self.produtos.remove(nome)
            self.historico.empilhar(("remover", nome))
            print(f"Produto '{nome}' removido")
        else:
            print("Produto não encontrado")
    
    def desfazer(self):
        acao = self.historico.desempilhar()
        
        if acao is None:
            return
        
        tipo, nome = acao
        
        if tipo == "adicionar":
            self.produtos.remove(nome)
            print(f"Desfeito: adição de '{nome}'")
        
        elif tipo == "remover":
            self.produtos.append(nome)
            print(f"Desfeito: remoção de '{nome}'")
    
    def mostrar_produtos(self):
        print("\nEstoque atual:")
        if len(self.produtos) == 0:
            print("Vazio")
        else:
            for p in self.produtos:
                print("-", p)



sistema = SistemaEstoque()

sistema.adicionar_produto("Arroz")
sistema.adicionar_produto("Feijão")
sistema.adicionar_produto("Macarrão")

sistema.mostrar_produtos()

print("\n--- Removendo ---")
sistema.remover_produto("Feijão")

sistema.mostrar_produtos()

print("\n--- Desfazendo ---")
sistema.desfazer()
sistema.desfazer()

sistema.mostrar_produtos()

