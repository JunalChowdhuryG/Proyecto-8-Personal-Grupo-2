# **Proyecto 8: Integracion de metricas agiles**

## **Informacion Personal**
* **Alumno:** `Chowdhury Gomez, Junal Johir` `20200092K`  
* **Grupo:** `2`
* **Correo:** `junal.chowdhury.g@uni.pe` 
* **Repositorio Grupal:** [Grupo 2: Repositorio Grupal](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3)  

## **Descripcion:**
Este repositorio documenta mis contribuciones al **Proyecto 8: Integracion de metricas agiles: Burn-Down y Lead Time con Scripts Personalizados** del **Grupo 2** (Junal, Janio, Andres) para la asignatura **Desarrollo de Software CC3S2** en el periodo academico **25-1** (2025). El proyecto consiste en desarrollar un mini dashboard agil local que calcula metricas como burn-down y lead time a partir de un repositorio Git, usando scripts Python y Bash. 
## **Sprints**

### **Sprint 1**
#### Demostracion en video
[Sprint 1 (Dia 3: 8/06/2025) Grupo 2 Proyecto 8 ](https://www.youtube.com/watch?v=iJIAYbbfaYw)
#### **Mi Rol**
* Fui responsable de establecer la estructura fundamental del proyecto ([Issue #1](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/1))
* Desarrollar el codigo de `calcular_metricas.py`([Issue #4](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-4/issues/1))
* Ademas, cree cinco Issue en un [Epic: Sprint 1](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/6) para planificar el Sprint 1, garantizando una asignacion clara de tareas para el equipo

#### **Contribuciones**

1. **2025-06-06: Creacion de 5 Issues para Sprint 1**
    - **Descripcion**: Defini las tareas del Sprint 1 creando cinco issues en un [Epic: Sprint 1](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/6) para guiar al equipo
   - **Issues**:
     - [Issue #1: Inicializar repositorio y estructura](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/1)
     - [Issue #2: Implementar hook commit-msg](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/2)
     - [Issue #3: Crear script generar_kanban.sh inicial](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/3)
     - [Issue #4: Desarrollar calcular_metricas.py](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/4)
     - [Issue #5: Crear datos iniciales en issues.json](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/5)
   - **Estado**: Cerrados (9 jun 2025)

2. **2025-06-07: Inicializacion de la Estructura del Repositorio (Issue #1)**
   - **Descripcion**: Configure la estructura de carpetas y archivos iniciales para soportar el desarrollo modular, cumpliendo con [Issue #1](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/1)
   - **Rama**: `feature-estructura`
   - **Commit**: `c53fe9c` [feat[#1]: Inicializar repositorio y estructura](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/commit/c53fe9c8b64135b3f7241c423c8e3750053cf4c6)
     - ~50 lineas, creo `src/`, `tests/`, `.gitignore`, `README.md`, `requirements.txt`, `pytest.ini`, `issues.json`
   - **Pull Request**: [merge[#1]: Fusionar rama feature-estructura](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/pull/7)
     - Revisado por Andres y Janio, fusionado a `develop` el 9 jun 2025
   - **Estado**: Cerrado (9 jun 2025)
```
C:.
├───metricas
├───src
├───tests/
├───requirements.txt
├───README.md
└───.gitignore
```
- **Proposito de las carpetas (repositorio grupal)**:
  - `src/`: Contiene el codigo Python, como `calcular_metricas.py`, que desarrolle para parsear el historial de Git.
  - `metricas/`: Almacena los CSVs generados, como `commits.csv`, con datos de commits.
  - `tests/`: Preparado para pruebas unitarias con `pytest`
  - `.gitignore`: Excluye archivos como `__pycache__` y `.venv/`.
  - `requirements.txt`: Vacio, ya que usamos bibliotecas estandar


3. **2025-06-08: Desarrollo de calcular_metricas.py (Issue #4)**
   - **Descripcion**: Implemente `calcular_metricas.py` para parsear el historial de Git y generar un CSV, cumpliendo con [Issue #4](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/4)
   - **Rama**: `feature-calcular-metricas`
   - **Commit**: `1173ee8` [feat[#4]: Crear calcular_metricas.py con funciones registrar_git_log y escribir_csv](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/commit/1173ee8b23f27d7e236ff120d004986cdd4f35b3)
   - **Pull Request**: [merge[#4]: Fusionar rama feature-calcular-metricas a develop](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/pull/9)
     - Revisado por Janio y Andres, fusionado a `develop` el 9 jun 2025
   - **Estado**: Cerrado (9 jun 2025)

`src/calcular_metricas.py`:
```python
import subprocess
import csv

# funcion que captura el historial de commits de un repositorio git
def registrar_git_log():
    # captura la salida del comando git log
    try:
        historial = subprocess.run(
            ['git', 'log', '--pretty=format:%H|%ad|%s', '--date=iso'],
            capture_output=True, text=True, check=True
        )
        commits = []
        for line in historial.stdout.splitlines():
            commit_hash, date, message = line.split('|', 2)
            commits.append((commit_hash, date, message))
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
            writer.writerow(['commit_hash', 'fecha', 'tipo_de_issue'])
            for commit in commits:
                writer.writerow(commit)
    except IOError as e:
        print(f"error al escribir en el CSV: {e}")


# main
if __name__ == "__main__":
    commits = registrar_git_log()
    escribir_csv(commits)

```
- **Uso**: Sirve como base para calcular metricas agiles en futuros sprints 
- **Salida**: Genera `metricas/commits.csv`, por ejemplo:
  ```
    commit_hash,fecha,tipo_de_issue
    a066af57769041d809ecd9ccfdc49828b6e4979b,2025-06-07 14:37:26 -0500,merge[#2]: Fusionar rama feature-git-hook-commit-msg
    2badba24e6337acd8d0bd3623f259b69fd5f85b7,2025-06-07 14:16:43 -0500,feat[#2]: Implementar git hook commit-msg
    ....
  ```
- **Funcionalidad**: Lee el historial de commits de Git usando el comando `git log --pretty=format:%H|%ad|%s --date=iso` y genera un archivo CSV (`metricas/commits.csv`) con las columnas `commit_hash`, `fecha`, y `tipo_de_issue`
- **Estructura**:
  - **`registrar_git_log()`**: Ejecuta `git log`, parsea la salida en tuplas `(hash, fecha, mensaje)`, y maneja errores con `try-except`
  - **`escribir_csv()`**: Escribe los datos en un CSV
  - **Main**: Coordina la ejecucion de ambas funciones



#### Instrucciones para Ejecutar los Parches y el Script Python
Los parches en `branches/` (`feature-estructura.patch` y `feature-calcular-metricas.patch`) permiten recrear los cambios de mis ramas en el repositorio grupal, incluyendo la estructura de carpetas y el script `calcular_metricas.py`. A continuacion, se detalla como aplicarlos y ejecutar el script:

1. **Clonar el repositorio grupal**:
   ```bash
   git clone https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3.git
   cd Grupo-2-Practica-Calificada-3
   ```

2. **Crear una rama temporal para aplicar los parches**:
   ```bash
   git checkout -b temp-patches
   ```

3. **Copiar los parches desde mi repositorio personal**:
   - clona mi repositorio personal:
     ```bash
     git clone https://github.com/JunalChowdhuryG/Proyecto-8-Personal-Grupo-2.git
     cd Proyecto-8-Personal-Grupo-2
     ```
   - copia los parches al repositorio grupal:
     ```bash
     cp branches/feature-estructura-junal.patch ../Grupo-2-Practica-Calificada-3/
     cp branches/feature-calcular-metricas-junal.patch ../Grupo-2-Practica-Calificada-3/
     cd ../Grupo-2-Practica-Calificada-3
     ```

4. **Aplicar el parche de feature-estructura**:
   - este parche crea la estructura inicial del repositorio
   ```bash
   git apply feature-estructura-junal.patch
   git commit -m "feat[#1]: Aplicar parche feature-estructura"
   ```

5. **Aplicar el parche de feature-calcular-metricas y ejecutar el script**:
   - este parche agrega `src/calcular_metricas.py`
   ```bash
   git apply feature-calcular-metricas-junal.patch
   git commit -m "feat[#4]: Aplicar parche feature-calcular-metricas"
   ```
   - Configura el entorno Python:
     ```bash
     python -m venv .venv
     source .venv/Scripts/activate
     ```
   - Ejecuta el script:
     ```bash
     python src/calcular_metricas.py
     ```
   - **Resultado**: Genera `metricas/commits.csv` con el historial de commits del repositorio

6. **Verificar la salida**:
   - abre `metricas/commits.csv` para ver los datos generados, como:
    ```
    commit_hash,fecha,tipo_de_issue
    a066af57769041d809ecd9ccfdc49828b6e4979b,2025-06-07 14:37:26 -0500,merge[#2]: Fusionar rama feature-git-hook-commit-msg
    2badba24e6337acd8d0bd3623f259b69fd5f85b7,2025-06-07 14:16:43 -0500,feat[#2]: Implementar git hook commit-msg
    dd7f0d82c20723421cad478417c7fbbeac2290e4,2025-06-07 13:34:38 -0500,merge[#1]: Fusionar rama feature-estructura
    c53fe9c8b64135b3f7241c423c8e3750053cf4c6,2025-06-07 12:59:43 -0500,feat[#1]: Inicializar repositorio y estructura
    2f1a006a839a69846ad239df3e5848772a4ad9f1,2025-06-06 18:13:21 -0500,feat[#0]: Agregar plantilla de Historia Usuario
     ```