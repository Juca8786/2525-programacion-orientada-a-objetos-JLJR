
# ----------------- Notas para entrega (README breve que puede copiarse al repositorio) -----------------
#
# TÍTULO: Agenda Personal - Aplicación GUI en Tkinter
# DESCRIPCIÓN:
#   Aplicación educativa desarrollada como entrega de la asignatura.
#   Permite crear, listar y eliminar eventos con guardado en archivo local (events.json).
#
# INSTRUCCIONES DE USO:
# 1. (Opcional) Instalar tkcalendar para un selector de fecha más amigable:
#       pip install tkcalendar
# 2. Ejecutar el script:
#       python Agenda_Personal_Tkinter.py
# 3. Añadir eventos completando Fecha (YYYY-MM-DD), Hora (HH:MM) y Descripción.
# 4. Seleccionar un evento en la lista y presionar "Eliminar Evento Seleccionado" para borrarlo.
#
# PRUEBAS REALIZADAS:
# - Agregar eventos con fecha/hora válidas.
# - Intentar agregar con campos vacíos -> se muestra advertencia.
# - Agregar con formato de fecha/hora incorrecto -> se muestra error.
# - Eliminar evento con confirmación.
# - Persistencia en archivo events.json entre ejecuciones.
#
# COMENTARIO ACADÉMICO (para el entregable):
#   El código se ha diseñado de forma modular y comentada para facilitar la lectura y la evaluación.
#   Se priorizó el uso de widgets estándar de Tkinter y ttk, y se implementó una persistencia sencilla
#   con JSON para que el proyecto pueda evaluarse sin necesidad de bases de datos externas.
#
# POSIBLES EXTENSIONES (no obligatorias):
# - Implementar edición completa de eventos (modificar y guardar).
# - Añadir búsqueda/filtrado por fecha o palabra clave.
# - Integración con calendarios externos (Google Calendar) o exportación a CSV.
# - Mejorar la gestión del formato de hora con controles tipo TimePicker.
#
# FIN DEL ARCHIVO
