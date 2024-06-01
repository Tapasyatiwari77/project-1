# project-1
import tkinter as tk
from tkinter import messagebox
import math

class TicTacToe:
    def _init_(self, master):
        self.master = master
        self.master.title("Tic Tac Toe")
        self.board = [' ' for _ in range(9)]
        self.current_player = "X"
        self.buttons = []
        self.create_buttons()
    
    def create_buttons(self):
        for i in range(9):
            button = tk.Button(self.master, text="", font=("Arial", 20), width=5, height=2,
                               command=lambda i=i: self.on_button_click(i))
            button.grid(row=i // 3, column=i % 3)
            self.buttons.append(button)
    
    def on_button_click(self, index):
        if self.board[index] == " ":
            self.board[index] = self.current_player
            self.buttons[index].config(text=self.current_player)
            if self.check_winner(self.current_player):
                messagebox.showinfo("Tic Tac Toe", f"Player {self.current_player} wins!")
                self.reset_game()
            elif " " not in self.board:
                messagebox.showinfo("Tic Tac Toe", "It's a draw!")
                self.reset_game()
            else:
                self.current_player = "O" if self.current_player == "X" else "X"
                if self.current_player == "O":
                    self.ai_move()
    
    def reset_game(self):
        self.board = [' ' for _ in range(9)]
        for button in self.buttons:
            button.config(text="")
        self.current_player = "X"
    
    def check_winner(self, player):
        win_conditions = [(0, 1, 2), (3, 4, 5), (6, 7, 8), 
                          (0, 3, 6), (1, 4, 7), (2, 5, 8), 
                          (0, 4, 8), (2, 4, 6)]
        for a, b, c in win_conditions:
            if self.board[a] == self.board[b] == self.board[c] == player:
                return True
        return False
    
    def ai_move(self):
        best_score = -math.inf
        best_move = None
        for i in range(9):
            if self.board[i] == " ":
                self.board[i] = "O"
                score = self.minimax(self.board, 0, False)
                self.board[i] = " "
                if score > best_score:
                    best_score = score
                    best_move = i
        self.board[best_move] = "O"
        self.buttons[best_move].config(text="O")
        if self.check_winner("O"):
            messagebox.showinfo("Tic Tac Toe", "Player O wins!")
            self.reset_game()
        elif " " not in self.board:
            messagebox.showinfo("Tic Tac Toe", "It's a draw!")
            self.reset_game()
        else:
            self.current_player = "X"
    
    def minimax(self, board, depth, is_maximizing):
        if self.check_winner("O"):
            return 1
        elif self.check_winner("X"):
            return -1
        elif " " not in board:
            return 0
        
        if is_maximizing:
            best_score = -math.inf
            for i in range(9):
                if board[i] == " ":
                    board[i] = "O"
                    score = self.minimax(board, depth + 1, False)
                    board[i] = " "
                    best_score = max(score, best_score)
            return best_score
        else:
            best_score = math.inf
            for i in range(9):
                if board[i] == " ":
                    board[i] = "X"
                    score = self.minimax(board, depth + 1, True)
                    board[i] = " "
                    best_score = min(score, best_score)
            return best_score

if _name_ == "_main_":
    root = tk.Tk()
    game = TicTacToe(root)
    root.mainloop()
