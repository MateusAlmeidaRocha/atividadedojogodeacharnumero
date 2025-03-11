https://colab.research.google.com/drive/19ggAVYT4rZax1_WM5tmM_XWWNVK6skoQ?usp=sharing

se não conseguir acessar o  link eu vou mandar o codico aqui:
import random
import matplotlib.pyplot as plt

class GameAgent:
    def __init__(self, difficulty='médio'):
        self.difficulty = difficulty
        self.attempts = 0
        self.history = []
        self.state = "Esperando tentativa"
        self.setup_difficulty()

    def setup_difficulty(self):
        if self.difficulty == 'facil':
            self.secret_number = random.randint(1, 100)
            self.max_attempts = 7
        elif self.difficulty == 'dificil':
            self.secret_number = random.randint(1, 100)
            self.max_attempts = 3
        else:
            self.secret_number = random.randint(1, 100)
            self.max_attempts = 5

    def make_guess(self, guess):
        self.attempts += 1
        self.history.append(guess)

        if guess == self.secret_number:
            self.state = "Acertou!"
            return "Parabéns! Você acertou o número."
        elif self.attempts >= self.max_attempts:
            self.state = "Fim do jogo"
            return f"Game Over! O número era {self.secret_number}."
        elif guess < self.secret_number:
            self.state = "Tentativa errada (muito baixo)"
            return "O número é maior. Tente novamente."
        else:
            self.state = "Tentativa errada (muito alto)"
            return "O número é menor. Tente novamente."

    def get_hint(self):
        if self.difficulty != 'facil':
            return "Dica não disponível para este nível de dificuldade, Se quiser dica coloca na dificuldade Facil."

        if not self.history:
            return f"Comece chutando um número próximo da metade do intervalo. O número secreto é {'par' if self.secret_number % 2 == 0 else 'ímpar'}."

        last_guess = self.history[-1]
        if last_guess < self.secret_number:
            return f"Tente um número um pouco maior. O número secreto é {'par' if self.secret_number % 2 == 0 else 'ímpar'}."
        else:
            return f"Tente um número um pouco menor. O número secreto é {'par' if self.secret_number % 2 == 0 else 'ímpar'}."

def plot_attempts(agent):
    plt.figure(figsize=(8, 5))
    plt.plot(range(1, len(agent.history) + 1), agent.history, marker='o', linestyle='-')
    plt.axhline(y=agent.secret_number, color='r', linestyle='--', label='Número Secreto')
    plt.xlabel("Tentativas")
    plt.ylabel("Valor do Palpite")
    plt.title("Evolução das Tentativas do Jogador")
    plt.legend()
    plt.show()


difficulty = input("Escolha a dificuldade (facil, medio, dificil): ").lower()
agent = GameAgent(difficulty=difficulty)

while agent.state not in ["Acertou!", "Fim do jogo"]:
    guess = int(input("Digite um número: "))
    print(agent.make_guess(guess))
    if agent.state not in ["Acertou!", "Fim do jogo"]:
        print(agent.get_hint())

plot_attempts(agent)
