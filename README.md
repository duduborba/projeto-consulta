import tkinter as tk
from tkinter import messagebox


class Consulta:
    def __init__(self, paciente, data, hora):
        self.paciente = paciente
        self.data = data
        self.hora = hora
        self.atendido = False


class HospitalApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Sistema de Agendamento Hospitalar")

        self.pacientes = []

        self.label_nome = tk.Label(root, text="Nome do Paciente:")
        self.label_nome.grid(row=0, column=0, padx=10, pady=10)
        self.entry_nome = tk.Entry(root)
        self.entry_nome.grid(row=0, column=1, padx=10, pady=10)

        self.label_data = tk.Label(root, text="Data da Consulta:")
        self.label_data.grid(row=1, column=0, padx=10, pady=10)
        self.entry_data = tk.Entry(root)
        self.entry_data.grid(row=1, column=1, padx=10, pady=10)

        self.label_hora = tk.Label(root, text="Hora da Consulta:")
        self.label_hora.grid(row=2, column=0, padx=10, pady=10)
        self.entry_hora = tk.Entry(root)
        self.entry_hora.grid(row=2, column=1, padx=10, pady=10)

        self.btn_agendar = tk.Button(root, text="Agendar Consulta", command=self.agendar_consulta)
        self.btn_agendar.grid(row=3, column=0, columnspan=2, pady=10)

        self.btn_ver_pacientes = tk.Button(root, text="Ver Pacientes", command=self.ver_pacientes)
        self.btn_ver_pacientes.grid(row=4, column=0, columnspan=2, pady=10)

        self.btn_detalhes_pacientes = tk.Button(root, text="Detalhes dos Pacientes", command=self.detalhes_pacientes)
        self.btn_detalhes_pacientes.grid(row=5, column=0, columnspan=2, pady=10)

        self.lista_consultas = tk.Listbox(root)
        self.lista_consultas.grid(row=6, column=0, columnspan=2, pady=10)

        self.btn_atender = tk.Button(root, text="Marcar como Atendido", command=self.marcar_atendido)
        self.btn_atender.grid(row=7, column=0, columnspan=2, pady=10)

    def agendar_consulta(self):
        paciente = self.entry_nome.get()
        data = self.entry_data.get()
        hora = self.entry_hora.get()

        if paciente and data and hora:
            nova_consulta = Consulta(paciente, data, hora)
            self.pacientes.append(nova_consulta)

            messagebox.showinfo("Sucesso", "Consulta agendada com sucesso!")

            # Limpar os campos após agendar a consulta
            self.entry_nome.delete(0, tk.END)
            self.entry_data.delete(0, tk.END)
            self.entry_hora.delete(0, tk.END)
        else:
            messagebox.showwarning("Erro", "Por favor, preencha todos os campos.")

    def ver_pacientes(self):
        self.lista_consultas.delete(0, tk.END)
        for i, consulta in enumerate(self.pacientes, start=1):
            status = "Atendido" if consulta.atendido else "Pendente"
            self.lista_consultas.insert(tk.END,
                                        f"{i}. {consulta.paciente} - {consulta.data} {consulta.hora} ({status})")

    def detalhes_pacientes(self):
        detalhes_window = tk.Toplevel(self.root)
        detalhes_window.title("Detalhes dos Pacientes")

        for consulta in self.pacientes:
            status = "Atendido" if consulta.atendido else "Pendente"
            detalhes_label = tk.Label(detalhes_window,
                                      text=f"{consulta.paciente} - {consulta.data} {consulta.hora} ({status})")
            detalhes_label.pack()

    def marcar_atendido(self):
        selected_index = self.lista_consultas.curselection()
        if selected_index:
            selected_index = int(selected_index[0])
            consulta = self.pacientes[selected_index]
            consulta.atendido = True
            messagebox.showinfo("Sucesso", f"Consulta de {consulta.paciente} marcada como atendida.")
            self.ver_pacientes()
        else:
            messagebox.showwarning("Erro", "Selecione um paciente para marcar como atendido.")


if __name__ == "__main__":
    root = tk.Tk()
    app = HospitalApp(root)
    root.mainloop()
