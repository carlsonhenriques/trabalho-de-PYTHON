from tkinter import Tk, Frame, TOP, StringVar, SOLID, LEFT, RIGHT, X, Y, Label, Entry, Button, Scrollbar, HORIZONTAL, VERTICAL, BOTTOM, NO, W
import sqlite3
import tkinter.ttk as ttk


root = Tk()
root.title("Exemplo de pesquisa")
width = 500
height = 400
screen_width = root.winfo_screenwidth()
screen_height = root.winfo_screenheight()
x = (screen_width/2) - (width/2)
y = (screen_height/2) - (height/2)
root.geometry("%dx%d+%d+%d" % (width, height, x, y))
root.resizable(0, 0)


def base_de_dados():
    conn = sqlite3.connect("./python.db")
    cursor = conn.cursor()
    cursor.execute(
        "CREATE TABLE IF NOT EXISTS `membros` (mem_id INTEGER NOT NULL PRIMARY KEY  AUTOINCREMENT, matricula TEXT, nome TEXT, av1 TEXT, av2 TEXT, av3 TEXT, avd TEXT, avds TEXT, email TEXT, endereço TEXT, campus TEXT, periodo TEXT   )")
    cursor.execute("SELECT * FROM `membros`")
    if cursor.fetchone() is None:
        cursor.execute(
            "INSERT INTO `membros` (matricula, nome, av1, av2, av3, avd, avds, email, endereço, campus, periodo) VALUES('001', 'felipe', '5', '7', '9', '6', '8', 'felipe@gmail.com', 'Rua das couves nuemro, 50', 'barra', '7' )")
        cursor.execute(
            "INSERT INTO `membros` (matricula, nome, av1, av2, av3, avd, avds, email, endereço, campus, periodo) VALUES('002', 'joao', '10', '7', '9', '5', '6', 'joao@gmail.com', 'Rua das orquidias nuemro, 10', 'barra', '7' )")
        cursor.execute(
            "INSERT INTO `membros` (matricula, nome, av1, av2, av3, avd, avds, email, endereço, campus, periodo) VALUES('003', 'maria', '5', '6', '9', '8', '5', 'maria@gmail.com', 'Rua itamarati nuemro, 16', 'recreio', '4' )")
        cursor.execute(
            "INSERT INTO `membros` (matricula, nome, av1, av2, av3, avd, avds, email, endereço, campus, periodo) VALUES('004', 'guilherme', '4', '7', '9', '8', '7', 'gui@gmail.com', 'Rua visconde nuemro, 89', 'caxia', '3' )")
        cursor.execute(
            "INSERT INTO `membros` (matricula, nome, av1, av2, av3, avd, avds, email, endereço, campus, periodo) VALUES('005', 'otavio', '10', '7', '9', '6', '9', 'otavio@gmail.com', 'Rua campos nuemro, 53', 'meier', '7' )")
        cursor.execute(
            "INSERT INTO `membros` (matricula, nome, av1, av2, av3, avd, avds, email, endereço, campus, periodo) VALUES('006', 'miriam', '10', '7', '6', '6', '6', 'miriam@gmail.com', 'Rua sales nuemro, 23', 'niteroi', '5' )")
        cursor.execute(
            "INSERT INTO `membros` (matricula, nome, av1, av2, av3, avd, avds, email, endereço, campus, periodo) VALUES('007', 'gustavo', '10', '9', '9', '7', '3', 'gustavo@gmail.com', 'Rua guimaraes, nuemro 76', 'nova iguacu', '2' )")
        cursor.execute(
            "INSERT INTO `membros` (matricula, nome, av1, av2, av3, avd, avds, email, endereço, campus, periodo) VALUES('008', 'pedro', '5', '7', '9', '6', '7', 'pedro@gmail.com', 'Rua das couves nuemro, 1005', 'recreio', '1' )")
        conn.commit()

    cursor.execute("SELECT * FROM `membros` ORDER BY `email` ASC")
    fetch = cursor.fetchall()
    for data in fetch:
        tree.insert('', 'end', values=(data))
    cursor.close()
    conn.close()


def pesquisar():
    if procura.get() != "":
        tree.delete(*tree.get_children())
        conn = sqlite3.connect("./python.db")
        cursor = conn.cursor()
        cursor.execute("SELECT * FROM `membros` WHERE `nome` LIKE ? OR `email` LIKE ?",
                       ('%'+str(procura.get())+'%', '%'+str(procura.get())+'%'))
        fetch = cursor.fetchall()
        for data in fetch:
            tree.insert('', 'end', values=(data))
        cursor.close()
        conn.close()


def limpa():
    conn = sqlite3.connect("./python.db")
    cursor = conn.cursor()
    tree.delete(*tree.get_children())
    cursor.execute("SELECT * FROM `membros` ORDER BY `email` ASC")
    fetch = cursor.fetchall()
    for data in fetch:
        tree.insert('', 'end', values=(data))
    cursor.close()
    conn.close()



procura = StringVar()


Top = Frame(root, width=500, bd=1, relief=SOLID)
Top.pack(side=TOP)
TopFrame = Frame(root, width=500)
TopFrame.pack(side=TOP)
TopForm = Frame(TopFrame, width=300)
TopForm.pack(side=LEFT, pady=10)
TopMargin = Frame(TopFrame, width=260)
TopMargin.pack(side=LEFT)
MidFrame = Frame(root, width=500)
MidFrame.pack(side=TOP)


lbl_title = Label(Top, width=500, font=('arial', 18),
                  text="Exemplo de filtro")
lbl_title.pack(side=TOP, fill=X)


procura = Entry(TopForm, textvariable=procura)
procura.pack(side=LEFT)


btn_procura = Button(TopForm, text="Pesquisar", bg="#006dcc", command=pesquisar)
btn_procura.pack(side=LEFT)
btn_limpar = Button(TopForm, text="Limpar", command=limpa)
btn_limpar.pack(side=LEFT)

scrollbarx = Scrollbar(MidFrame, orient=HORIZONTAL)
scrollbary = Scrollbar(MidFrame, orient=VERTICAL)
tree = ttk.Treeview(MidFrame, columns=("membrosID", "matricula", "nome", "av1", "av2", "av3", "avd", "avds", "email", "endereço", "campus", "periodo"),
                    selectmode="extended", height=400, yscrollcommand=scrollbary.set, xscrollcommand=scrollbarx.set)
scrollbary.config(command=tree.yview)
scrollbary.pack(side=RIGHT, fill=Y)
scrollbarx.config(command=tree.xview)
scrollbarx.pack(side=BOTTOM, fill=X)
tree.heading('membrosID', text="membrosID", anchor=W)
tree.heading('matricula', text="matricula", anchor=W)
tree.heading('nome', text="nome", anchor=W)
tree.heading('av1', text="av1", anchor=W)
tree.heading('av2', text="av2", anchor=W)
tree.heading('av3', text="av3", anchor=W)
tree.heading('avd', text="avd", anchor=W)
tree.heading('avds', text="avds", anchor=W)
tree.heading('email', text="email", anchor=W)
tree.heading('endereço', text="endereço", anchor=W)
tree.heading('campus', text="campus", anchor=W)
tree.heading('periodo', text="periodo", anchor=W)
tree.column('#0', stretch=NO, minwidth=0, width=0)
tree.column('#1', stretch=NO, minwidth=0, width=0)
tree.column('#2', stretch=NO, minwidth=0, width=80)
tree.column('#3', stretch=NO, minwidth=0, width=120)
tree.column('#4', stretch=NO, minwidth=0, width=170)
tree.column('#5', stretch=NO, minwidth=0, width=80)
tree.column('#6', stretch=NO, minwidth=0, width=80)
tree.column('#7', stretch=NO, minwidth=0, width=80)
tree.column('#8', stretch=NO, minwidth=0, width=80)
tree.column('#9', stretch=NO, minwidth=0, width=80)
tree.column('#10', stretch=NO, minwidth=0, width=80)
tree.pack()


if __name__ == '__main__':
    base_de_dados()
    root.mainloop()
