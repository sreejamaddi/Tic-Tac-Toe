import tkinter as tk
from tkinter import messagebox

class TicTacToe:
    def __init__(self, root):
        self.root = root
        self.root.title("✨ Tic-Tac-Toe Game")
        self.root.configure(bg="#2e2e2e")

        self.current_player = "X"
        self.board = [["" for _ in range(3)] for _ in range(3)]
        self.buttons = [[None for _ in range(3)] for _ in range(3)]

        self.create_widgets()

    def create_widgets(self):
        title = tk.Label(self.root, text="Tic-Tac-Toe", font=("Helvetica", 28, "bold"),
                         fg="#ffffff", bg="#2e2e2e")
        title.pack(pady=10)

        self.frame = tk.Frame(self.root, bg="#2e2e2e")
        self.frame.pack()

        for r in range(3):
            for c in range(3):
                button = tk.Button(
                    self.frame, text="", font=('Arial', 36, 'bold'), width=4, height=2,
                    bg="#444", fg="#ffffff", activebackground="#666", bd=0,
                    command=lambda row=r, col=c: self.on_click(row, col)
                )
                button.grid(row=r, column=c, padx=5, pady=5)
                self.buttons[r][c] = button

        self.status_label = tk.Label(self.root, text="Player X's Turn", font=("Arial", 18, "bold"),
                                     fg="#00ffcc", bg="#2e2e2e")
        self.status_label.pack(pady=15)

        self.reset_button = tk.Button(self.root, text="🔄 Restart Game", font=("Arial", 14),
                                      bg="#00b894", fg="white", activebackground="#00cec9",
                                      relief="flat", command=self.reset_game)
        self.reset_button.pack(pady=5)

    def on_click(self, row, col):
        if self.buttons[row][col]["text"] == "" and not self.check_winner():
            self.buttons[row][col]["text"] = self.current_player
            self.board[row][col] = self.current_player

            if self.check_winner():
                self.status_label.config(text=f"🎉 Player {self.current_player} Wins!", fg="#feca57")
                self.highlight_winner()
                messagebox.showinfo("Game Over", f"Player {self.current_player} wins!")
            elif self.check_draw():
                self.status_label.config(text="It's a Draw!", fg="#fab1a0")
                messagebox.showinfo("Game Over", "It's a draw!")
            else:
                self.current_player = "O" if self.current_player == "X" else "X"
                self.status_label.config(text=f"Player {self.current_player}'s Turn",
                                         fg="#00ffcc" if self.current_player == "X" else "#ff7675")

    def check_winner(self):
        b = self.board
        self.winning_cells = []

        for i in range(3):
            if b[i][0] == b[i][1] == b[i][2] != "":
                self.winning_cells = [(i, 0), (i, 1), (i, 2)]
                return True
            if b[0][i] == b[1][i] == b[2][i] != "":
                self.winning_cells = [(0, i), (1, i), (2, i)]
                return True

        if b[0][0] == b[1][1] == b[2][2] != "":
            self.winning_cells = [(0, 0), (1, 1), (2, 2)]
            return True
        if b[0][2] == b[1][1] == b[2][0] != "":
            self.winning_cells = [(0, 2), (1, 1), (2, 0)]
            return True

        return False

    def highlight_winner(self):
        for r, c in self.winning_cells:
            self.buttons[r][c].config(bg="#ffeaa7", fg="#2d3436")

    def check_draw(self):
        return all(self.board[r][c] != "" for r in range(3) for c in range(3))

    def reset_game(self):
        self.board = [["" for _ in range(3)] for _ in range(3)]
        self.current_player = "X"
        self.status_label.config(text="Player X's Turn", fg="#00ffcc")
        for r in range(3):
            for c in range(3):
                self.buttons[r][c].config(text="", bg="#444", fg="#ffffff")


if __name__ == "__main__":
    root = tk.Tk()
    app = TicTacToe(root)
    root.mainloop()
