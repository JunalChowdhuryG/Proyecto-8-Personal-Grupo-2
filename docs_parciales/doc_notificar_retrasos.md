# Documentacion de `notificar_retrasos.py`

## Descripcion
El script `notificar_retrasos.py` es una contribucion al **Proyecto 8: Integracion de Metricas agiles** del **Grupo 2** para la asignatura **Desarrollo de Software CC3S2** (periodo 25-1, 2025). Desarrollado en **Sprint 3** ([Issue #26](https://github.com/JunalChowdhuryG/Grupo-2-Practica-Calificada-3/issues/26)), este script identifica issues con un *lead time* superior a un umbral (72 horas) y genera correos simulados para notificar a los responsables

## Funcionalidad
El script realiza dos tareas principales:
1. **Deteccion de retrasos** (`notificar_retrasos`):
   - Lee `metricas/lead_time.csv` para obtener *lead times* de issues.
   - Valida el formato CSV (`issue_id`, `lead_time_hours`).
   - Compara cada *lead time* con el umbral (72 horas).
   - Para issues retrasados, obtiene el propietario desde `issues.json` y genera un correo simulado.
   - Escribe correos en `reports/emails/issue_<id>_delay.txt`, creando el directorio si no existe.
2. **Obtencion del propietario** (`get_owner`):
   - Busca el campo `owner` en `issues.json` para el `issue_id` especificado.
   - Devuelve `"unknown"` si no se encuentra.

## Codigo
```python
import csv
import os
import json


def notificar_retrasos(archivo_lead_time='metricas/lead_time.csv', archivo_issues='issues.json', umbral_horas=72):
    # detectar problemas con el tiempo de entrega mayor al umbral y generar email
    if not os.path.exists(archivo_lead_time):
        print(f"error: {archivo_lead_time} no encontrado")
        return
    if not os.path.exists(archivo_issues):
        print(f"error: {archivo_issues} no encontrado")
        return
    try:
        # cargar issues
        with open(archivo_issues, 'r') as f:
            issues = json.load(f)
        # leer lead times
        with open(archivo_lead_time, 'r') as f:
            lector = csv.DictReader(f)
            if not lector.fieldnames or 'issue_id' not in lector.fieldnames or 'lead_time_hours' not in lector.fieldnames:
                print(f"error: formato csv erroneo en {archivo_lead_time}")
                return
            issues_retrasados = []
            total_issues = 0
            for row in lector:
                try:
                    issue_id = row['issue_id']
                    lead_time = float(row['lead_time_hours'])
                    total_issues += 1
                    print(f"Issue #{issue_id}: {lead_time: .1f} horas", end="")
                    if lead_time > umbral_horas:
                        issues_retrasados.append((issue_id, lead_time))
                        owner = get_owner(issue_id, issues)
                        if owner == "unknown":
                            print(f"owner no encontrado en el issue #{issue_id}")
                            continue
                        directorio_email_issue = f"reports/emails/issue_{issue_id}_delay.txt"
                        # Crear directorio si no existe
                        directorio_email = 'reports/emails'
                        os.makedirs(directorio_email, exist_ok=True)
                        if os.path.exists(directorio_email_issue):
                            print(f"\nemail para issue #{issue_id} ya existe")
                            continue
                        # genera email
                        contenido_email = f"""From: devops@local
To: {owner}@local
Subject: Retraso en issue #{issue_id}

Hola {owner}, \nEl issue #{issue_id} lleva un lead time de {round(lead_time, 2)} horas, superior al umbral de {umbral_horas} horas. Por favor, revisar estado.
"""
                        with open(directorio_email_issue, 'w') as f:
                            f.write(contenido_email)
                        print(f"\nemail generado: {directorio_email_issue}")
                    else:
                        print("\nOK")
                except (ValueError, KeyError) as e:
                    print(f"\nerror al procesar issue {row.get('issue_id', 'unknown')}: {e}")
    except (IOError, csv.Error, json.JSONDecodeError) as e:
        print(f"\nerror al leer los archivos: {e}")


def get_owner(issue_id, issues):
    # buscar el owner del issue de la lista de issues
    for issue in issues:
        if str(issue['id']) == str(issue_id):
            return issue['owner']
    return "unknown"


if __name__ == "__main__":
    umbral = 72
    notificar_retrasos(umbral_horas=umbral)
```

## Entradas
- **metricas/lead_time.csv**: Generado por `calcular_metricas.py`, con columnas:
  - `issue_id`: ID del issue
  - `lead_time_hours`: *Lead time* en horas
  Ejemplo:
  ```
  issue_id,lead_time_hours
  1,1.18
  ```
- **issues.json**: Archivo JSON con datos de issues, incluyendo:
  - `id`: ID del issue
  - `owner`: Propietario del issue
  Ejemplo:
  ```json
    [
    {
        "id": 1,
        "title": "Inicializar repositorio y estructura",
        "state": "closed",
        "created_at": "2025-06-19T12:25:00",
        "closed_at": "2025-06-19T13:36:00",
        "owner": "junal"
    }
    ]
  ```

## Salidas
- **reports/emails/issue_<id>_delay.txt**: Correos simulados para issues con *lead time* > 72 horas. Ejemplo:
  ```
  From: devops@local
  To: junal@local
  Subject: Retraso en issue #1

  Hola junal,
  El issue #1 lleva un lead time de 80.0 horas, superior al umbral de 72 horas. Por favor, revisar estado.
  ```
- **Consola**: Mensajes indicando el estado de cada issue (`Issue #1: 1.18 horas OK` o `Email generado: reports/emails/issue_1_delay.txt`)

## Ejecucion
```bash
cd Proyecto-8-Personal-Grupo-2
python -m venv .venv
source .venv/Scripts/activate
cd scripts-personal
python src/notificar_retrasos.py
```
Para simular un retraso:
```bash
echo "issue_id,lead_time_hours" > metricas/lead_time.csv
echo "1,80" >> metricas/lead_time.csv
python src/notificar_retrasos.py
cat reports/emails/issue_1_delay.txt
```

## Notas
- Usa bibliotecas estandar (`csv`, `os`, `json`), sin install