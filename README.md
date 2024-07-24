# Gestor de Tareas con SQLite

Bienvenido a este sofisticado y romántico gestor de tareas. En el corazón de este proyecto yace una base de datos SQLite que organiza tus tareas de manera eficiente y segura. Este script en Python te permitirá agregar, mostrar, editar y eliminar tareas con facilidad.

## Características

- **Gestión de Tareas**: Agrega, muestra, edita y elimina tareas.
- **Base de Datos SQLite**: Utiliza SQLite para almacenar tus tareas de manera persistente.
- **Interfaz Sencilla**: Interactúa con el script a través de una interfaz de línea de comandos fácil de usar.

## Código

```python
import sqlite3

# Crear conexión a la base de datos
conn = sqlite3.connect('tareas.db')
cursor = conn.cursor()

# Crear tabla (si no existe)
cursor.execute('''
CREATE TABLE IF NOT EXISTS tasks (
    id INTEGER PRIMARY KEY,
    task TEXT NOT NULL,
    status TEXT NOT NULL
)
''')
conn.commit()

# Función para agregar tarea
def add_task(task):
    try:
        cursor.execute('INSERT INTO tasks (task, status) VALUES (?, ?)', (task, 'pending'))
        conn.commit()
        print("Tarea agregada exitosamente.")
    except sqlite3.Error as e:
        print(f"Error al agregar la tarea: {e}")

# Función para mostrar tareas
def show_tasks():
    try:
        cursor.execute('SELECT * FROM tasks')
        tasks = cursor.fetchall()
        if tasks:
            print("Lista de tareas:")
            for task in tasks:
                print(f"ID: {task[0]}, Tarea: {task[1]}, Estado: {task[2]}")
        else:
            print("No hay tareas para mostrar.")
    except sqlite3.Error as e:
        print(f"Error al mostrar las tareas: {e}")

# Función para editar tarea
def edit_task(id, task, status):
    try:
        cursor.execute('UPDATE tasks SET task=?, status=? WHERE id=?', (task, status, id))
        conn.commit()
        print("Tarea editada exitosamente.")
    except sqlite3.Error as e:
        print(f"Error al editar la tarea: {e}")

# Función para eliminar tarea
def delete_task(id):
    try:
        cursor.execute('DELETE FROM tasks WHERE id=?', (id,))
        conn.commit()
        print("Tarea eliminada exitosamente.")
    except sqlite3.Error as e:
        print(f"Error al eliminar la tarea: {e}")

# Función principal
def main():
    while True:
        print("\n1. Agregar tarea")
        print("2. Mostrar tareas")
        print("3. Editar tarea")
        print("4. Eliminar tarea")
        print("5. Salir")
        choice = input("Ingrese una opción: ")
        if choice == '1':
            task = input("Ingrese la tarea: ")
            add_task(task)
        elif choice == '2':
            show_tasks()
        elif choice == '3':
            id = input("Ingrese el ID de la tarea: ")
            new_task = input("Ingrese la nueva tarea: ")
            new_status = input("Ingrese el nuevo estado: ")
            edit_task(id, new_task, new_status)
        elif choice == '4':
            id = input("Ingrese el ID de la tarea: ")
            delete_task(id)
        elif choice == '5':
            print("¡Hasta luego!")
            break
        else:
            print("Opción inválida. Intente nuevamente.")

if __name__ == '__main__':
    main()
```

## Uso

1. **Instalación**: Asegúrate de tener Python instalado. No se necesitan librerías externas.
2. **Ejecuta el Script**: Abre tu terminal o línea de comandos y navega hasta el directorio donde se encuentra el script.
3. **Interacción**: Sigue las instrucciones en pantalla para agregar, mostrar, editar y eliminar tareas.

## Ejemplo

```sh
$ python gestor_tareas.py

1. Agregar tarea
2. Mostrar tareas
3. Editar tarea
4. Eliminar tarea
5. Salir
Ingrese una opción: 1
Ingrese la tarea: Comprar leche
Tarea agregada exitosamente.

1. Agregar tarea
2. Mostrar tareas
3. Editar tarea
4. Eliminar tarea
5. Salir
Ingrese una opción: 2
Lista de tareas:
ID: 1, Tarea: Comprar leche, Estado: pending
```
