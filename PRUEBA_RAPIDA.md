# Prueba rápida (≈10 minutos, Windows)

Objetivo: comprobar que el proyecto corre en tu computadora. Marca cada paso y avisa si algo falla.

> **Importante:** esta carpeta trae los resultados de la corrida de demostración real
> (`datos/configuracion_aprobada.json`, `salidas/`, `reporte_ciclo_supervision.md`, `corrida_demostracion.log`).
> Cada vez que ejecutes `sigma_crew.py` **se sobrescriben**. Descomprime el `.zip` en una carpeta de
> prueba y no uses la copia que vayan a entregar.

## 1. Requisitos

- **Python 3.12** (no 3.13 ni 3.14). Comprueba con `py -3.12 --version`.
- *Opcional, para la prueba con IA real:* [Ollama](https://ollama.com) instalado. Un modelo de 7B
  necesita unos 6 GB de memoria libre (mejor con GPU de 8 GB; en CPU funciona pero tarda mucho más).

## 2. Instalar (una sola vez)

Abre PowerShell dentro de esta carpeta y ejecuta:

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python generar_telemetria.py
```

Si PowerShell bloquea el script de activación: `Set-ExecutionPolicy -Scope Process Bypass` y vuelve a intentar.

## 3. Prueba A — sin IA (stub), 1 minuto

```powershell
python sigma_crew.py
```

Debe terminar sin errores y, al final, imprimir una línea como:
`Verificacion del reporte: configuracion publicada; 0 cifra(s) sin respaldo; ...`

Resultado esperado: se crean `datos/configuracion_aprobada.json` y `reporte_ciclo_supervision.md`.
(Este modo usa un guion fijo: solo prueba que el código funciona.)

## 4. Prueba B — con IA real (Ollama), 3 a 12 minutos

```powershell
ollama pull qwen2.5:7b
ollama create sigma-qwen25 -f Modelfile.qwen25
$env:SIGMA_LLM="ollama"
python sigma_crew.py
```

Resultado esperado: el equipo ejecuta las 6 tareas y, la mayoría de las veces, publica una
configuración. **Es normal que a veces no publique**: el modelo es pequeño y no responde igual cada vez;
en ese caso el reporte lo dice con un aviso «NO SE PUBLICO NINGUNA CONFIGURACION».

## 5. Qué nos interesa que reportes

1. ¿Cuál fue tu versión de Python, tu GPU/CPU y tu memoria?
2. ¿Prueba A funcionó? ¿Prueba B funcionó? ¿Cuánto tardó?
3. Si hubo un error, copia las últimas 20 líneas de la consola.
4. ¿Se publicó la configuración (existe `datos/configuracion_aprobada.json`)?

## 6. Notebook (opcional)

Abre `SIGMA_Vial_Fase3.ipynb` en VS Code o Jupyter. Ya trae las salidas de la corrida de demostración
(no hace falta ejecutarlo). Para ejecutarlo de nuevo: kernel con Python 3.12 y las mismas
dependencias; el modelo se elige con `SIGMA_LLM`.
