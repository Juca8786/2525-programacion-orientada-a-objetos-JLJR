"""
Aplicación de Lista de Tareas (To-Do) usando Tkinter
Archivo: tkinter_todo_app.py
Descripción: Interfaz simple para añadir, marcar como completadas y eliminar tareas.
Autor: Juca Rodriguez Jose Luis
Fecha: 2025-09-24

Instrucciones:
- Ejecuta: python tkinter_todo_app.py
- Haz enter para añadir una tarea después de escribirla.
- Haz doble clic sobre una tarea para marcarla / desmarcarla.
- Selecciona una tarea y usa los botones "Marcar como Completada" o "Eliminar Tarea".

Este archivo está comentado para explicar la lógica y manejo de eventos.
"""

import tkinter as tk
from tkinter import ttk, messagebox
import tkinter.font as tkfont


class TodoApp(tk.Tk):
    """Ventana principal de la aplicación To-Do."""

    def __init__(self):
        super().__init__()
        self.title("Lista de Tareas")
        self.geometry("480x420")
        self.resizable(False, False)

        # Estado interno: lista de dicts con 'text' y 'completed'
        self.tasks = []

        # Fuentes: normal y con tachado para tareas completadas
        base_font = tkfont.nametofont("TkDefaultFont")
        self.font_normal = base_font.copy()
        self.font_strike = base_font.copy()
        self.font_strike.configure(overstrike=1)

        # Crear la UI
        self._create_widgets()
        self._layout_widgets()
        self._bind_events()

    def _create_widgets(self):
        # Entrada para nueva tarea
        self.entry_task = ttk.Entry(self, font=self.font_normal)

        # Botones
        self.btn_add = ttk.Button(self, text="Añadir Tarea", command=self.add_task)
        self.btn_toggle = ttk.Button(self, text="Marcar como Completada", command=self.toggle_selected_task)
        self.btn_delete = ttk.Button(self, text="Eliminar Tarea", command=self.delete_selected_task)

        # Listbox para mostrar tareas y scrollbar
        self.frame_list = ttk.Frame(self)
        self.scrollbar = ttk.Scrollbar(self.frame_list, orient=tk.VERTICAL)
        self.listbox = tk.Listbox(
            self.frame_list,
            yscrollcommand=self.scrollbar.set,
            selectmode=tk.SINGLE,
            font=self.font_normal,
            activestyle='none',
            highlightthickness=0,
            selectbackground='#cce5ff'
        )
        self.scrollbar.config(command=self.listbox.yview)

        # Etiqueta de ayuda
        self.label_help = ttk.Label(self, text="Escribe una tarea y presiona Enter o " +
                                             "usa 'Añadir Tarea'. Doble clic para alternar completada.")

    def _layout_widgets(self):
        padding = 10
        self.entry_task.pack(fill=tk.X, padx=padding, pady=(padding, 0))

        btn_frame = ttk.Frame(self)
        btn_frame.pack(fill=tk.X, padx=padding, pady=(8, 0))

        self.btn_add.pack(in_=btn_frame, side=tk.LEFT, expand=True, fill=tk.X)
        self.btn_toggle.pack(in_=btn_frame, side=tk.LEFT, expand=True, fill=tk.X, padx=(6, 6))
        self.btn_delete.pack(in_=btn_frame, side=tk.LEFT, expand=True, fill=tk.X)

        self.frame_list.pack(fill=tk.BOTH, expand=True, padx=padding, pady=(12, 0))
        self.listbox.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)
        self.scrollbar.pack(side=tk.RIGHT, fill=tk.Y)

        self.label_help.pack(fill=tk.X, padx=padding, pady=(8, padding))

    def _bind_events(self):
        # Enter en el campo de texto añade la tarea
        self.entry_task.bind('<Return>', lambda event: self.add_task())

        # Doble clic en la lista alterna completado
        self.listbox.bind('<Double-Button-1>', lambda event: self._on_double_click(event))

        # Tecla Supr (Delete) elimina la tarea seleccionada
        self.bind('<Delete>', lambda event: self.delete_selected_task())

    # ------------------ Lógica de tareas ------------------
    def add_task(self):
        """Añade una nueva tarea desde el Entry. Ignora cadenas vacías."""
        text = self.entry_task.get().strip()
        if not text:
            messagebox.showinfo("Aviso", "La tarea está vacía. Escribe algo antes de añadir.")
            return

        # Añadir al estado interno
        self.tasks.append({'text': text, 'completed': False})

        # Refrescar la vista
        self._refresh_listbox()

        # Limpiar la entrada y mantener el foco para seguir escribiendo (flujo de trabajo humano)
        self.entry_task.delete(0, tk.END)
        self.entry_task.focus()

    def toggle_selected_task(self):
        """Marca o desmarca la tarea seleccionada como completada."""
        index = self._get_selected_index()
        if index is None:
            messagebox.showinfo("Aviso", "Selecciona una tarea para marcarla como completada.")
            return

        self.tasks[index]['completed'] = not self.tasks[index]['completed']
        self._refresh_listbox()

    def delete_selected_task(self):
        """Elimina la tarea seleccionada."""
        index = self._get_selected_index()
        if index is None:
            messagebox.showinfo("Aviso", "Selecciona una tarea para eliminarla.")
            return

        # Confirmación humana: evita borrados accidentales
        answer = messagebox.askyesno("Confirmar eliminación", f"¿Eliminar la tarea:\n\n{self.tasks[index]['text']}?")
        if not answer:
            return

        del self.tasks[index]
        self._refresh_listbox()

    def _get_selected_index(self):
        sel = self.listbox.curselection()
        if not sel:
            return None
        return sel[0]

    def _on_double_click(self, event):
        # Alterna la tarea doble clickeada
        widget = event.widget
        index = widget.nearest(event.y)
        if index is None:
            return
        self.tasks[index]['completed'] = not self.tasks[index]['completed']
        self._refresh_listbox()

    def _refresh_listbox(self):
        """Repinta todos los elementos de la lista respetando el estado 'completed'."""
        self.listbox.delete(0, tk.END)
        for i, task in enumerate(self.tasks):
            self.listbox.insert(tk.END, task['text'])
            # Aplicar estilo: si está completada, usar fuente tachada
            if task['completed']:
                self.listbox.itemconfig(i, font=self.font_strike)
            else:
                self.listbox.itemconfig(i, font=self.font_normal)


if __name__ == '__main__':
    app = TodoApp()
    app.mainloop()

