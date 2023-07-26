import os
import tkinter as tk
from tkinter import filedialog
import matplotlib.pyplot as plt
from collections import Counter
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg


def listar_arquivos(diretorio):
    arquivos = os.listdir(diretorio)
    return arquivos
    
def procurar_diretorio():
    diretorio = filedialog.askdirectory()
    if diretorio:
        diretorio_selecionado.set(diretorio)
        exibir_lista()

def exibir_lista():
    diretorio = diretorio_selecionado.get()
    arquivos = listar_arquivos(diretorio)

    if not arquivos:
        resultado.config(text="Nenhum arquivo encontrado.")
    else:
        lista_arquivos.delete(0, tk.END)  # Limpa a lista de arquivos para evitar duplicatas
        for arquivo in arquivos:
            # Obtém apenas o nome do arquivo (sem o caminho)
            nome_arquivo = os.path.basename(arquivo)
            lista_arquivos.insert(tk.END, nome_arquivo)

def selecionar_todos():
    lista_arquivos.selection_set(0, tk.END)

def analisar_arquivos():
    arquivos_selecionados = lista_arquivos.curselection()
    diretorio = diretorio_selecionado.get()
    arquivos = listar_arquivos(diretorio)
    arquivos_para_analisar = [arquivos[i] for i in arquivos_selecionados]

    print("Diretório selecionado:", diretorio)
    print("Arquivos para análise:", arquivos_para_analisar)

    if not arquivos_para_analisar:
        resultado.config(text="Nenhum arquivo selecionado.")
        return

    tipos_analise_selecionadas = [tipo_analise.get() for tipo_analise in checkboxes_tipo_analise]

    resultado.config(text=f"Arquivos selecionados para análise: {' '.join(arquivos_para_analisar)}\n"
                         f"Tipos de análise selecionados: {' '.join(tipos_analise_selecionadas)}")

    if "Quantitativa" in tipos_analise_selecionadas:
        analise_quantitativa(arquivos_para_analisar)

    if "Exploratória" in tipos_analise_selecionadas:
        plt.clf()  # Limpa o conteúdo do gráfico para exibir um novo
        analise_exploratoria(arquivos_para_analisar)

    if "Qualitativa" in tipos_analise_selecionadas:
        plt.clf()  # Limpa o conteúdo do gráfico para exibir um novo
        analise_qualitativa(arquivos_para_analisar)

    # Aqui você pode adicionar outras análises conforme necessário

def analise_exploratoria(arquivos_para_analisar):
    # Implementação da análise exploratória...
    pass


def analise_quantitativa(arquivos_para_analisar):
    total_palavras = 0
    total_linhas = 0
    total_caracteres = 0

    for arquivo in arquivos_para_analisar:
        with open(arquivo, "r", encoding="utf-8") as f:
            conteudo = f.read()
            palavras = conteudo.split()
            total_palavras += len(palavras)
            total_linhas += conteudo.count('\n')
            total_caracteres += len(conteudo)

    quantidade_arquivos = len(arquivos_para_analisar)
    tamanho_medio_palavras = total_caracteres / total_palavras if total_palavras > 0 else 0
    media_linhas_por_arquivo = total_linhas / quantidade_arquivos if quantidade_arquivos > 0 else 0

    resultado.config(text=f"Quantidade de arquivos analisados: {quantidade_arquivos}\n"
                          f"Total de palavras: {total_palavras}\n"
                          f"Total de linhas: {total_linhas}\n"
                          f"Tamanho médio das palavras: {tamanho_medio_palavras:.2f}\n"
                          f"Média de linhas por arquivo: {media_linhas_por_arquivo:.2f}")

    # Limpa qualquer gráfico anterior antes de criar novos
    plt.clf()

    # Gráfico de barras para mostrar a quantidade total de palavras, linhas e tamanho médio das palavras
    plt.subplot(1, 3, 1)
    plt.bar(["Total Palavras"], [total_palavras])
    plt.title("Total de Palavras")

    plt.subplot(1, 3, 2)
    plt.bar(["Total Linhas"], [total_linhas])
    plt.title("Total de Linhas")

    plt.subplot(1, 3, 3)
    plt.bar(["Tamanho Médio"], [tamanho_medio_palavras])
    plt.title("Tamanho Médio das Palavras")

    plt.tight_layout()

    # Incorporando os gráficos na janela do tkinter
    canvas = FigureCanvasTkAgg(plt.gcf(), master=janela)
    canvas.draw()
    canvas.get_tk_widget().pack()

def analise_qualitativa(arquivos_para_analisar):
    palavras_contagem = Counter()

    diretorio = diretorio_selecionado.get()

    for arquivo in arquivos_para_analisar:
        caminho_completo = os.path.join(diretorio, arquivo)
        with open(caminho_completo, "r", encoding="utf-8") as f:
            palavras_contagem.update(f.read().split())

    palavras, ocorrencias = zip(*palavras_contagem.most_common(10))  # Vamos exibir as 10 palavras mais comuns

    # Limpa qualquer gráfico anterior antes de criar um novo
    plt.clf()

    # Criando o gráfico de pizza
    plt.figure(figsize=(8, 5))
    plt.pie(ocorrencias, labels=palavras, autopct="%1.1f%%")
    plt.title("Análise Qualitativa - Frequência de Palavras")

    # Incorporando o gráfico na janela do tkinter
    canvas = FigureCanvasTkAgg(plt.gcf(), master=janela)
    canvas.draw()

    # Exibir o resultado na interface
    resultado.config(text="Análise qualitativa concluída.")
    canvas.get_tk_widget().pack()



def analise_exploratoria(arquivos_para_analisar):
    palavras_contagem = Counter()

    diretorio = diretorio_selecionado.get()

    for arquivo in arquivos_para_analisar:
        caminho_completo = os.path.join(diretorio, arquivo)
        with open(caminho_completo, "r", encoding="utf-8") as f:
            palavras_contagem.update(f.read().split())

    palavras, ocorrencias = zip(*palavras_contagem.most_common(10))  # Vamos exibir as 10 palavras mais comuns

    # Limpa qualquer gráfico anterior antes de criar um novo
    plt.clf()

    # Criando o gráfico de barras
    plt.figure(figsize=(8, 5))
    plt.bar(palavras, ocorrencias)
    plt.xlabel("Palavras")
    plt.ylabel("Ocorrências")
    plt.title("Análise Exploratória - Palavras mais Comuns")
    plt.xticks(rotation=45)

    # Incorporando o gráfico na janela do tkinter
    canvas = FigureCanvasTkAgg(plt.gcf(), master=janela)
    canvas.draw()

    # Exibir o resultado na interface
    resultado.config(text="Análise exploratória concluída.")
    canvas.get_tk_widget().pack()


# Criando a janela
janela = tk.Tk()
janela.title("Análise de Arquivos")

# Variáveis para armazenar as seleções do usuário
diretorio_selecionado = tk.StringVar()

# Botão para procurar diretório
botao_procurar = tk.Button(janela, text="Procurar Diretório", command=procurar_diretorio)
botao_procurar.pack(pady=10)

# Listbox para mostrar a lista de arquivos
lista_arquivos = tk.Listbox(janela, selectmode=tk.MULTIPLE, width=50)
lista_arquivos.pack()

# Botão para selecionar todos os arquivos na lista
botao_selecionar_todos = tk.Button(janela, text="Selecionar Todos", command=selecionar_todos)
botao_selecionar_todos.pack(pady=5)

# Checkbuttons para selecionar o tipo de análise
tipo_analise_label = tk.Label(janela, text="Escolha o tipo de análise:")
tipo_analise_label.pack()

opcoes_analise = ["Qualitativa", "Quantitativa", "Exploratória"]

checkboxes_tipo_analise = []

for opcao in opcoes_analise:
    var = tk.StringVar()
    checkbox = tk.Checkbutton(janela, text=opcao, variable=var, onvalue=opcao, offvalue="")
    checkbox.pack()
    checkboxes_tipo_analise.append(var)

# Botão para iniciar a análise
botao_analisar = tk.Button(janela, text="Analisar", command=analisar_arquivos)
botao_analisar.pack(pady=10)

# Resultado da lista de arquivos ou mensagem de erro
resultado = tk.Label(janela, text="", justify="left")
resultado.pack()

janela.mainloop()

def fechar_janela():
    janela.destroy()

janela.protocol("WM_DELETE_WINDOW", fechar_janela)

def analise_quantitativa(arquivos_para_analisar):
    total_palavras = 0
    total_linhas = 0
    total_caracteres = 0

    for arquivo in arquivos_para_analisar:
        with open(arquivo, "r", encoding="utf-8") as f:
            conteudo = f.read()
            palavras = conteudo.split()
            total_palavras += len(palavras)
            total_linhas += conteudo.count('\n')
            total_caracteres += len(conteudo)

    quantidade_arquivos = len(arquivos_para_analisar)
    tamanho_medio_palavras = total_caracteres / total_palavras if total_palavras > 0 else 0
    media_linhas_por_arquivo = total_linhas / quantidade_arquivos if quantidade_arquivos > 0 else 0

    resultado.config(text=f"Quantidade de arquivos analisados: {quantidade_arquivos}\n"
                          f"Total de palavras: {total_palavras}\n"
                          f"Total de linhas: {total_linhas}\n"
                          f"Tamanho médio das palavras: {tamanho_medio_palavras:.2f}\n"
                          f"Média de linhas por arquivo: {media_linhas_por_arquivo:.2f}")

# ...

def analisar_arquivos():
    arquivos_selecionados = lista_arquivos.curselection()
    diretorio = diretorio_selecionado.get()
    arquivos = listar_arquivos(diretorio)
    arquivos_para_analisar = [arquivos[i] for i in arquivos_selecionados]

    if not arquivos_para_analisar:
        resultado.config(text="Nenhum arquivo selecionado.")
        return

    tipos_analise_selecionadas = [tipo_analise.get() for tipo_analise in checkboxes_tipo_analise]

    resultado.config(text=f"Arquivos selecionados para análise: {' '.join(arquivos_para_analisar)}\n"
                         f"Tipos de análise selecionados: {' '.join(tipos_analise_selecionadas)}")

    if "Quantitativa" in tipos_analise_selecionadas:
        analise_quantitativa(arquivos_para_analisar)

    if "Exploratória" in tipos_analise_selecionadas:
        plt.clf()  # Limpa o conteúdo do gráfico para exibir um novo
        analise_exploratoria(arquivos_para_analisar)

    # Aqui você pode adicionar outras análises conforme necessário


import os
import tkinter as tk
from tkinter import filedialog
import matplotlib.pyplot as plt
from collections import Counter
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg


def salvar_resultados():
    # Solicitar ao usuário que escolha um diretório para salvar os resultados
    diretorio_salvar = filedialog.askdirectory()

    if not diretorio_salvar:
        return  # O usuário cancelou a seleção do diretório

    # Criar um arquivo de texto com os resultados
    resultados = resultado.cget("text")
    nome_arquivo = "resultados_analise.txt"
    caminho_arquivo = os.path.join(diretorio_salvar, nome_arquivo)

    with open(caminho_arquivo, "w", encoding="utf-8") as f:
        f.write(resultados)

    # Mostrar uma mensagem de sucesso
    resultado.config(text="Resultados salvos com sucesso!")
    
def salvar_graficos():
    # Solicitar ao usuário que escolha um diretório para salvar os gráficos
    diretorio_salvar = filedialog.askdirectory()

    if not diretorio_salvar:
        return  # O usuário cancelou a seleção do diretório

    # Salvar o gráfico da análise quantitativa
    plt.savefig(os.path.join(diretorio_salvar, "analise_quantitativa.png"))

    # Salvar o gráfico da análise exploratória
    plt.savefig(os.path.join(diretorio_salvar, "analise_exploratoria.png"))

    # Salvar o gráfico da análise qualitativa
    plt.savefig(os.path.join(diretorio_salvar, "analise_qualitativa.png"))

    # Opcionalmente, você pode adicionar outros gráficos para salvar, se necessário.


# Criando a janela
janela = tk.Tk()
janela.title("Análise de Arquivos")

# Variáveis para armazenar as seleções do usuário
diretorio_selecionado = tk.StringVar()

# Restante do código (definição dos outros widgets: botão "Procurar Diretório", lista de arquivos, checkboxes, botão "Analisar", etc)...

# Botão para selecionar todos os arquivos na lista
botao_selecionar_todos = tk.Button(janela, text="Selecionar Todos", command=selecionar_todos)
botao_selecionar_todos.pack(pady=5)

# Botão para salvar os gráficos
botao_salvar_graficos = tk.Button(janela, text="Salvar Gráficos", command=salvar_graficos)
botao_salvar_graficos.pack(pady=10)

# Executar o loop principal de eventos
janela.mainloop()
