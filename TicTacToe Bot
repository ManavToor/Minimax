import time


class Board:
    # represents the Tic-Tac-Tow game board, X moves first
    def __init__(self, board=None):
        # 2D array representing the board
        #    A     B    C
        # 1 [' ', ' ', ' '],
        # 2 [' ', ' ', ' '],
        # 3 [' ', ' ', ' ']
        self.board = board
        if board is None:
            self.board = [[' ', ' ', ' '],
                          [' ', ' ', ' '],
                          [' ', ' ', ' ']]

    # Takes string coordinate and translates into an index position for the board array
    def coord_to_index(self, coord):
        column = coord[0]
        row = coord[1]

        if column == 'A':
            column = 0
        elif column == 'B':
            column = 1
        elif column == 'C':
            column = 2

        return (int(row) - 1), column

    # returns which players' turn it is, X goes first
    def whos_turn(self):
        num_of_x = 0
        num_of_o = 0

        for y in self.board:
            for x in y:
                if x == 'X':
                    num_of_x += 1
                elif x == 'O':
                    num_of_o += 1

        if num_of_x - num_of_o == 0:
            return 'X'
        return 'O'

    # places a token on the board
    def take_turn(self, location):
        index = self.coord_to_index(location)

        self.board[index[0]][index[1]] = self.whos_turn()

    # prints the board
    def __repr__(self):
        return f"  {self.board[0][0]} | {self.board[0][1]} | {self.board[0][2]} \n" \
               f"----|---|----\n" \
               f"  {self.board[1][0]} | {self.board[1][1]} | {self.board[1][2]} \n" \
               f"----|---|----\n" \
               f"  {self.board[2][0]} | {self.board[2][1]} | {self.board[2][2]} \n"


class Bot:
    def __init__(self, player):
        self.player = player

    # tic tac toe bot, soon to be AI (hopefully)
    # Default set to 'O', use MIN function when using as 'X'

    # Takes string coordinate and translates into an index position for the board array
    def coord_to_index(self, coord):
        column = coord[0]
        row = coord[1]

        if column == 'A':
            column = 0
        elif column == 'B':
            column = 1
        elif column == 'C':
            column = 2

        return (int(row)), column

    # Takes index position and translates it into a string for the player
    def index_to_coord(self, index):
        column = index[1]
        row = index[0]

        if column == 0:
            column = 'A'
        elif column == 1:
            column = 'B'
        elif column == 2:
            column = 'C'

        return f"{column}{row + 1}"

    # returns all possible moves
    def actions(self, board):
        moves = list()

        for i in range(0, 3):
            for j in range(0, 3):
                if board.board[i][j] == ' ':
                    moves.append([i, j])

        for move in moves:
            moves[moves.index(move)] = self.index_to_coord(move)

        return moves

    # returns new tic-tac-toe board representing game board after a move
    def result(self, action, board):
        import copy
        new_board = Board(copy.deepcopy(board.board))
        new_board.take_turn(action)
        return new_board

    # returns true or false for if the game is over
    def terminal(self, board):
        # check if board is out of empty spaces
        filled = bool
        for y in board.board:
            for x in y:
                if x == ' ':
                    filled = False
        if filled:
            return True

        # check for 3 in a row horizontally
        for y in board.board:
            if (y[0] == y[1] and y[1] == y[2] and y[2] == 'X') or (y[0] == y[1] and y[1] == y[2] and y[2] == 'O'):
                return True

        # check for 3 in a row vertically (X)
        for i in range(0, 3):
            win = True
            for y in board.board:
                if y[i] != 'X':
                    win = False
            if win:
                return True

        # check for 3 in a row vertically (O)
        for i in range(0, 3):
            win = True
            for y in board.board:
                if y[i] != 'O':
                    win = False
            if win:
                return True

        # check for 3 in a row diagonally (down)
        if (board.board[0][0] == board.board[1][1] and board.board[1][1] == board.board[2][2] and
            board.board[2][2] == 'X') or (
                board.board[0][0] == board.board[1][1] and board.board[1][1] == board.board[2][2] and
                board.board[2][2] == 'O'):
            return True
        # check for 3 in a row diagonally (up)
        if (board.board[0][2] == board.board[1][1] and board.board[1][1] == board.board[2][0] and board.board[2][
            0] == 'X') or (board.board[0][2] == board.board[1][1] and board.board[1][1] == board.board[2][0] and
                           board.board[2][0] == 'O'):
            return True

    # assigns score -1 if O looses, 0 for tie, 1 if O wins
    # inverted if bot is 'X'
    def utility(self, board):
        # check for 3 in a row horizontally
        for y in board.board:
            if y[0] == y[1] and y[1] == y[2] and y[2] == 'X':
                return -1 if (self.player == 'O') else 1
            elif y[0] == y[1] and y[1] == y[2] and y[2] == 'O':
                return 1 if (self.player == 'O') else -1

        # check for 3 in a row vertically (X)
        for i in range(0, 3):
            win = True
            for y in board.board:
                if y[i] != 'X':
                    win = False
            if win:
                return -1 if (self.player == 'O') else 1

        # check for 3 in a row vertically (O)
        for i in range(0, 3):
            win = True
            for y in board.board:
                if y[i] != 'O':
                    win = False
            if win:
                return 1 if (self.player == 'O') else -1

        # check for 3 in a row diagonally (down) (X)
        if board.board[0][0] == board.board[1][1] and board.board[1][1] == board.board[2][2] and board.board[2][2] == 'X':
            return -1 if (self.player == 'O') else 1
        # check for 3 in a row diagonally (down) (O)
        if board.board[0][0] == board.board[1][1] and board.board[1][1] == board.board[2][2] and board.board[2][2] == 'O':
            return 1 if (self.player == 'O') else -1
        # check for 3 in a row diagonally (up) (X)
        if board.board[2][0] == board.board[1][1] and board.board[1][1] == board.board[0][2] and board.board[0][2] == 'X':
            return -1 if (self.player == 'O') else 1
        # check for 3 in a row diagonally (up) (O)
        if board.board[2][0] == board.board[1][1] and board.board[1][1] == board.board[0][2] and board.board[0][2] == 'O':
            return 1 if (self.player == 'O') else -1

        # check if board is out of empty spaces
        filled = bool
        for y in board.board:
            for x in y:
                if x == ' ':
                    filled = False
        if filled:
            return 0

    # Recursive functions that apply Minimax algorithm to return best move
    def MAX(self, board):
        if self.terminal(board):
            return self.utility(board)
        score = 100
        for action in self.actions(board):
            score = min(score, self.MIN(self.result(action, board)))
        return score

    def MIN(self, board):
        if self.terminal(board):
            return self.utility(board)

        score = -100
        for action in self.actions(board):
            score = max(score, self.MAX(self.result(action, board)))
        return score


def game_over(score):
    if score == 1:
        print("YOU LOOSE!!! LMAOOOO :p")
        quit()
    elif score == 0:
        print("TIE -_-")
        quit()
    else:
        print("you win  i guess :(")
        quit()


# user goes first
def play_first():
    my_board = Board()
    bot = Bot('O')

    while True:
        print(my_board)

        my_move = input("Make your move: ")
        my_board.take_turn(my_move)

        print(my_board)
        time.sleep(2)

        if bot.terminal(my_board):
            game_over(bot.utility(my_board))

        actions = bot.actions(my_board)

        score = -10
        move = actions[0]
        for action in actions:
            action_score = bot.MAX(bot.result(action, my_board))
            if action_score > score:
                score = action_score
                move = action

        my_board.take_turn(move)

        if bot.terminal(my_board):
            game_over(bot.utility(my_board))


# bot goes first
def play_second():
    my_board = Board()
    bot = Bot('X')

    while True:
        while True:
            print(my_board)

            time.sleep(2)

            actions = bot.actions(my_board)

            score = -10
            move = actions[0]

            for action in actions:
                action_score = bot.MAX(bot.result(action, my_board))
                if action_score > score:
                    score = action_score
                    move = action

            my_board.take_turn(move)

            if bot.terminal(my_board):
                game_over(bot.utility(my_board))

            print(my_board)

            my_move = input("Make your move: ")

            my_board.take_turn(my_move)

            if bot.terminal(my_board):
                game_over(bot.utility(my_board))


turn = input("First (F) or second (S)? ")

if turn == 'F':
    play_first()
else:
    play_second()
