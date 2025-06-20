# Documentacion de `calcular_metricas.py`
## Descripcion
El script `calcular_metricas.py` es una contribucion clave al **Proyecto 8: Integracion de Metricas agiles** del **Grupo 2** para la asignatura **Desarrollo de Software CC3S2** (periodo 25-1, 2025). Este script calcula metricas agiles (*burn-down* y *lead time*) a partir de un repositorio Git, generando archivos CSV con datos de commits y tiempos de resolucion de issues. Desarrollado en **Sprint 1** ([Issue #4](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/4)), mejorado con *lead time* en **Sprint 2** ([Issue #15](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/15)), y optimizado en **Sprint 3** ([Issue #28](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/28)).
## Funcionalidad
El script realiza tres tareas principales:
1. **Captura del historial de commits** (`registrar_git_log`):
   - Ejecuta `git log --pretty=format:%H|%ad|%s --date=iso` para obtener commits.
   - Parsea cada commit en hash, fecha, mensaje, y dias desde el primer commit.
   - Usa expresiones regulares para clasificar mensajes (e.g., `feat[#4]`, `merge[#7]`).
2. **Generacion de CSV de commits** (`escribir_csv`):
   - Escribe datos de commits en `metrics/commits.csv` con columnas: `commit_hash`, `fecha`, `tipo_de_issue`, `dia_desde_inicio`.
3. **Calculo de *lead time*** (`calcular_lead_time`):
   - Lee `issues.json` para obtener issues cerrados.
   - Calcula el *lead time* (horas entre creacion y cierre) y lo guarda en `metrics/lead_time.csv` con columnas: `issue_id`, `lead_time_hours`.


## Codigo

```python
import subprocess
import csv
import json
from datetime import datetime
import re

# funcion que captura el historial de commits de un repositorio git
def registrar_git_log():
    # captura la salida del comando git log
    try:
        historial = subprocess.run(
            ['git', 'log', '--pretty=format:%H|%ad|%s', '--date=iso'],
            capture_output=True, text=True, check=True
        )
        commits = []
        primera_fecha = None
        for line in historial.stdout.splitlines():
            commit_hash, date, message = line.split('|', 2)
            fecha_commit = datetime.fromisoformat(date.replace('Z', '+00:00'))
            if primera_fecha is None:
                primera_fecha = fecha_commit
            dia_desde_inicio = (fecha_commit - primera_fecha).days
            match = re.match(r'^(feat|fix|merge|docs|update|test|refactor)\[#(\d+)\]', message)
            tipo = match.group(1) + f"[#{match.group(2)}]" if match else message
            commits.append((commit_hash, date, tipo, dia_desde_inicio))
        return commits
    except subprocess.CalledProcessError as e:
        print(f"error al ejecutar el comando git log: {e}")
        return []


# funcion que escribe los datos de los commits en un archivo CSV
def escribir_csv(commits, output_file='metricas/commits.csv'):
    # escribe los datos en un archivo CSV
    try:
        with open(output_file, 'w', newline='') as f:
            writer = csv.writer(f)
            writer.writerow(['commit_hash', 'fecha', 'tipo_de_issue','dia_desde_inicio'])
            for commit in commits:
                writer.writerow(commit)
    except IOError as e:
        print(f"error al escribir en el CSV: {e}")


# funcion que calcula el lead time de los issues cerrados
def calcular_lead_time(issues_file='issues.json', output_file='metricas/lead_time.csv'):
    try:
        # se lee el archivo JSON con los issues
        with open(issues_file, 'r') as f:
            issues = json.load(f)
        lead_times = []
        for issue in issues:
            # se calcula el lead time solo para issues cerrados
            if issue['state'] == 'closed' and issue['closed_at']:
                # se convierte las fechas de creacion y cierre a objetos datetime
                f_creado = datetime.fromisoformat(issue['created_at'].replace('Z', '+00:00'))
                f_cerrado = datetime.fromisoformat(issue['closed_at'].replace('Z', '+00:00'))
                # se calcula el lead time en horas
                lead_time_horas = (f_cerrado - f_creado).total_seconds() / 3600
                # se guarda
                lead_times.append((issue['id'], lead_time_horas))
        # se escribe  resultado en un archivo CSV
        with open(output_file, 'w', newline='') as f:
            writer = csv.writer(f)
            writer.writerow(['issue_id', 'lead_time_hours'])
            for lead_time in lead_times:
                writer.writerow(lead_time)
        return lead_times
    except (IOError, json.JSONDecodeError, ValueError) as e:
        print(f"error al callcular lead time: {e}")
        return []


# main
if __name__ == "__main__":
    # se registra los commits
    commits = registrar_git_log()
    # se escribe los commits en un archivo CSV
    escribir_csv(commits)
    # se calcula el lead time de los issues cerrados
    calcular_lead_time()
```


## Entradas
- **Historial de Git**: Obtenido mediante `git log` en el directorio actual.
- **issues.json**: Archivo JSON con datos de issues (`id`, `state`, `created_at`, `closed_at`, `owner`).

## Salidas
- **metrics/commits.csv**: Contiene:
  - `commit_hash`: Identificador del commit.
  - `fecha`: Fecha en formato ISO.
  - `tipo_de_issue`: Mensaje del commit, clasificado (e.g., `feat[#4]`) o sin procesar.
  - `dia_desde_inicio`: Dias desde el primer commit.
  Ejemplo:
  ```
    commit_hash,fecha,tipo_de_issue,dia_desde_inicio
    ca97fa484427d075cf3747922285b919c31a486a,2025-06-10 18:39:17 -0500,Agregar calcular_metricas.py,0
    ed277ba8e39b63b5dc7f3aabe13527b5e962bbe8,2025-06-10 18:38:51 -0500,Agregar patch feature-estructura-junal.patch,-1
  ```
- **metrics/lead_time.csv**: Contiene:
  - `issue_id`: ID del issue.
  - `lead_time_hours`: Tiempo en horas desde creacion hasta cierre.
  Ejemplo:
  ```
  issue_id,lead_time_hours
  1,1.18
  ```

## Ejecucion
```bash
cd Proyecto-8-Personal-Grupo-2
python -m venv .venv
source .venv/Scripts/activate
python scripts-personal/src/calcular_metricas.py
```

## Notas
- Usa bibliotecas estandar (`subprocess`, `csv`, `json`, `datetime`, `re`), sin dependencias externas
- Maneja errores con `try-except` para robustez (e.g., fallos en `git log`, lectura de JSON, escritura de CSV)
- Optimizado en Sprint 3 ([Issue #28](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/28)) para mejorar el parseo de mensajes 
- Verifica la salida con:
  ```bash
  cat metrics/commits.csv
  cat metrics/lead_time.csv
  ```