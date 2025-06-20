# **Proyecto 8: Integracion de metricas agiles**

## **Informacion Personal**
* **Alumno:** `Chowdhury Gomez, Junal Johir` `20200092K`  
* **Grupo:** `2`
* **Correo:** `junal.chowdhury.g@uni.pe` 
* **Repositorio Grupal:** [Grupo 2: Repositorio Grupal](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3)  
* **Repositorio Individual**: [https://github.com/JunalChowdhuryG/Proyecto-8-Personal-Grupo-2](https://github.com/JunalChowdhuryG/Proyecto-8-Personal-Grupo-2)

## **Descripcion:**
Este repositorio documenta mis contribuciones al **Proyecto 8: Integracion de metricas agiles: Burn-Down y Lead Time con Scripts Personalizados** del **Grupo 2** (Junal, Janio, Andres) para la asignatura **Desarrollo de Software CC3S2** en el periodo academico **25-1** (2025). El proyecto consiste en desarrollar un mini dashboard agil local que calcula metricas como *burn-down* y *lead time* a partir de un repositorio Git, utilizando scripts Python y Bash. Mis tareas incluyeron la inicializacion del repositorio, desarrollo y optimizacion de scripts (`calcular_metricas.py`, `notificar_retrasos.py`), y planificacion de sprints

## Mi Rol
Fui lider de desarrollo y coordinador de sprints, responsable de:
- **Sprint 1**: Configurar la estructura del repositorio ([Issue #1](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/1)) y desarrollar `calcular_metricas.py` ([Issue #4](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/4)).
- **Sprint 2**: Implementar el calculo de *lead time* en `calcular_metricas.py` ([Issue #15](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/15)).
- **Sprint 3**: Crear `notificar_retrasos.py` para notificar retrasos ([Issue #26](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/26)) y optimizar `calcular_metricas.py` ([Issue #28](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/28)).
- **Planificacion**: Crear issues para los sprints:
  - [Epic: Sprint 1](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/6))
  - [Epic: Sprint 2](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/24))
  - [Epic: Sprint 3](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/30))


## Estructura del Repositorio
```
/Proyecto-8-Personal-Grupo-2/
├── README.md
├── CONTRIBUTIONS.md
├── branches/
│   ├── sprint-1-feature-estructura-junal.patch
│   ├── sprint-1-feature-calcular-metricas-junal.patch
│   ├── sprint-2-feature-lead-time-junal.patch
│   ├── sprint-3-feature-notificar-retrasos-junal.patch
│   ├── sprint-3-feature-optimizar-calcular-metricas-junal.patch
├── scripts-personal/
│   ├── metricas/
│   ├── reports/     
│   ├── src/
│        ├── calcular_metricas.py
│        └── notificar_retrasos.py
├── videos/
│   └── videos-sprints.md
├── docs_parciales/
│   ├── doc_calcular_metricas.md
│   └── doc_final_summary-junal.md
```

- **branches/**: Contiene parches que reflejan mis contribuciones, con formato `<sprint>-feature-<task>-junal.patch`.
- **src/**: Scripts Python que desarrolle.
- **videos/**: Videos de demostracion por sprint, mostrando ejecucion de scripts y resultados.
- **docs_parciales/**: Documentacion parcial de mis aportes

## Comprobacion de Funcionalidad
Para verificar la funcionalidad de mis scripts (`calcular_metricas.py`, `notificar_retrasos.py`) sin aplicar parches, usa los scripts en `scripts-personal/src/`. Estos pasos asumen que `src/`, `metricas/`, y `reports/emails/` estan presentes en el repositorio clonado

1. **Clonar el repositorio individual**:
   ```bash
   git clone https://github.com/JunalChowdhuryG/Proyecto-8-Personal-Grupo-2.git
   cd Proyecto-8-Personal-Grupo-2
   cd scripts-personal
   ```

2. **Configurar el entorno Python**:
   ```bash
   python -m venv .venv
   source .venv/Scripts/activate
   ```
   No se requieren dependencias externas, ya que los scripts usan bibliotecas estandar

3. **Ejecutar `calcular_metricas.py`**:
   ```bash
   python src/calcular_metricas.py
   ```
   **Salida**: Genera `metricas/commits.csv` (commit hash, fecha, mensaje, dias desde inicio) y `metricas/lead_time.csv` (issue, *lead time* en horas). Ejemplo:
   - `metricas/commits.csv`:
     ```
     commit_hash,fecha,tipo_de_issue,dia_desde_inicio
     ca97fa484427d075cf3747922285b919c31a486a,2025-06-10 18:39:17 -0500,Agregar calcular_metricas.py,0
     ed277ba8e39b63b5dc7f3aabe13527b5e962bbe8,2025-06-10 18:38:51 -0500,Agregar patch feature-estructura-junal.patch,-1
     a1d67a0abd4c1412f714822125ae31a03e599c9b,2025-06-10 18:37:48 -0500,Agregar patch feature-estructura-junal.patch,-1
     ```
   - `metricas/lead_time.csv`:
     ```
     issue_id,lead_time_hours
     1,1.18
     ```
   Verifica con:
   ```bash
   cat metricas/commits.csv
   cat metricas/lead_time.csv
   ```

4. **Ejecutar `notificar_retrasos.py`**:
   ```bash
   python src/notificar_retrasos.py
   ```
   **Salida**:
     ```
     Issue #1: 1.18 horas
     OK
     ```
   - Para verificar la generacion de correos, simula un retraso (>72 horas) en `metricas/lead_time.csv`:
     ```bash
     echo "issue_id,lead_time_hours" > metricas/lead_time.csv
     echo "1,80" >> metricas/lead_time.csv
     ```
   - Vuelve a ejecutar:
     ```bash
     python src/notificar_retrasos.py
     ```
     **Salida**:
     ```
     Issue #1: 80.0 horas
     Email generado: reports/emails/issue_1_delay.txt
     ```
     Verifica el correo:
     ```bash
     cat reports/emails/issue_1_delay.txt
     ```
     **Contenido esperado**:
     ```
     From: devops@local
     To: junal@local
     Subject: Retraso en issue #1

     Hola junal,
     El issue #1 lleva un lead time de 80.0 horas, superior al umbral de 72 horas. Por favor, revisar estado.
     ```

## Instrucciones de Uso
1. **Clonar el repositorio individual**:
   ```bash
   git clone https://github.com/JunalChowdhuryG/Proyecto-8-Personal-Grupo-2.git
   cd Proyecto-8-Personal-Grupo-2
   ```

2. **Aplicar parches**:
   Inicializa un repositorio Git para aplicar los parches:
   ```bash
   git init
   git commit --allow-empty -m "Inicializar repositorio para parches"
   ```
   Aplica los parches en orden:
   ```bash
   git apply branches/sprint-1-feature-estructura-junal.patch
   git commit -m "feat[#1]: Aplicar parche feature-estructura"
   git apply branches/sprint-1-feature-calcular-metricas-junal.patch
   git commit -m "feat[#4]: Aplicar parche feature-calcular-metricas"
   git apply branches/sprint-2-feature-lead-time-junal.patch
   git commit -m "feat[#15]: Aplicar parche feature-lead-time"
   git apply branches/sprint-3-feature-notificar-retrasos-junal.patch
   git commit -m "feat[#26]: Aplicar parche feature-notificar-retrasos"
   git apply branches/sprint-3-feature-optimizar-calcular-metricas-junal.patch
   git commit -m "feat[#28]: Aplicar parche feature-optimizar-calcular-metricas"
   ```
   **Nota**: Si hay conflictos debido a fusiones con rebase en el repositorio grupal, se usa `git apply --reject` y resuelve manualmente los archivos `.rej`.

3. **Configurar el entorno Python**:
   ```bash
   python -m venv .venv
   source .venv/Scripts/activate
   ```
   No se requieren dependencias externas, ya que los scripts usan bibliotecas estandar

4. **Ejecutar scripts**:
   - Generar metricas:
     ```bash
     python src/calcular_metricas.py
     ```
     **Salida**: Crea `metricas/commits.csv` y `metricas/lead_time.csv` con datos de commits y *lead time*.
   - Detectar retrasos:
     ```bash
     python src/notificar_retrasos.py
     ```
     **Salida**: Genera correos simulados en `reports/emails/` para issues con *lead time* > 72 horas.

5. **Verificar resultados**:
   ```bash
   cat metricas/commits.csv
   cat metricas/lead_time.csv
   cat reports/emails/issue_1_delay.txt
   ```

6. **Ver videos de demostracion**:
   Videos en `videos/` muestran la ejecucion de scripts y resultados por sprint. Links actualizados en [CONTRIBUTIONS.md](CONTRIBUTIONS.md).

## Contribuciones
Para detalles de mis contribuciones, incluyendo commits, pull requests, y videos, consulta [CONTRIBUTIONS.md](CONTRIBUTIONS.md). Resumen:
- **Sprint 1**: Estructura del repositorio, `calcular_metricas.py`, planificacion de issues
- **Sprint 2**: Calculo de *lead time*, planificacion de issues
- **Sprint 3**: Notificaciones de retrasos, optimizacion de metricas, planificacion de issues

## Notas
- Los parches reflejan commits atomicos con mensajes descriptivos, cumpliendo con la rubrica
- Los scripts en `src/` son versiones finales, incluyendo optimizaciones de Sprint 3
- Los videos estan pendientes de carga; los links en `CONTRIBUTIONS.md` seran actualizados