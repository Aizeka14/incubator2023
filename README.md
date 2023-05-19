from tkinter import *
from random import choice

frm = [];
btn = []
playArea = [];
nMoves = 0


def play(n):
    global nMoves
    nMoves += 1
    if nMoves == 1:
        i = 0
        while i < 40:
            j = choice(range(0, 256))
            if j != n and playArea[j] != -1:
                playArea[j] = -1
                i += 1
        for i in range(0, 256):
            if playArea[i] != -1:
                k = 0
                if i not in range(0, 256, 16):
                    if playArea[i - 1] == -1: k += 1
                    if i > 15:
                        if playArea[i - 17] == -1: k += 1
                    if i < 240:
                        if playArea[i + 15] == -1: k += 1
                if i not in range(-1, 256, 16):
                    if playArea[i + 1] == -1: k += 1
                    if i > 15:
                        if playArea[i - 15] == -1: k += 1
                    if i < 240:
                        if playArea[i + 17] == -1: k += 1
                if i > 15:
                    if playArea[i - 16] == -1: k += 1
                if i < 240:
                    if playArea[i + 16] == -1: k += 1
                playArea[i] = k

    btn[n].config(text=playArea[n], state=DISABLED)
    if playArea[n] == 0:
        btn[n].config(text=' ', bg='#ccb')
    elif playArea[n] == -1:
        if nMoves < (256 - 40):
            tk.title('Game over.')
            nMoves = 256
        for i in range(0, 256):
            if playArea[i] == -1:
                btn[i].config(text='\u26AB')
    if nMoves == (256 - 40):
        tk.title('Win!')


def marker(n):
    btn[n].config(text='\u2661')

tk = Tk()
tk.title('Let\'s play!')

for i in range(0, 16):
    frm.append(Frame())
    frm[i].pack(expand=YES, fill=BOTH)
    for j in range(0, 16):
        btn.append(Button(frm[i], text=' ',
                          font=('mono', 16, 'bold'),
                          width=2, height=1,
                          command=lambda n=i * 16 + j: play(n)))
        btn[i * 16 + j].pack(side=LEFT, expand=NO, fill=Y)
        btn[i * 16 + j].bind('<Button-3>', lambda event, n=i * 16 + j: marker(n))
        playArea.append(0)

def NewGame():
    global nMoves, btnBG, mrk
    nMoves = 0;
    mrk = 40
    for i in range(0, 256):
        playArea[i] = 0
        btn[i].config(text=' ', state=NORMAL)

Button(tk, text='New game', font=(16), command=NewGame).pack(side=LEFT, expand=YES, fill=Y)

mainloop()
