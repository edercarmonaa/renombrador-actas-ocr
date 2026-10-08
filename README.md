# renombrador-actas-ocr

Herramienta en Python para renombrar lotes de PDFs escaneados de actas de entrega
de termino de obra, cruzando la fecha y el nombre extraidos por OCR contra un
archivo Excel de control.

## Problema que resuelve

Cuando se reciben muchas actas escaneadas, renombrarlas manualmente exige abrir
cada PDF, leer la fecha del acta, identificar a la persona que entrega y buscar
el nombre correcto en una hoja de calculo. Ese proceso es lento, repetitivo y
facil de equivocar, especialmente si los nombres vienen con variaciones de OCR o
si el lote contiene documentos que no corresponden al tipo de acta esperado.

Este proyecto automatiza esa revision:

- valida que el PDF sea un `ACTA DE ENTREGA DE TERMINO DE OBRA`;
- extrae por OCR la fecha del acta en la primera hoja;
- extrae posibles nombres de entrega en la segunda hoja;
- compara esos datos contra un Excel;
- renombra el PDF con el valor definido en la columna correspondiente;
- genera un reporte de lo ocurrido para revisar coincidencias, omisiones y
  errores.

## Estado de privacidad

El repositorio esta preparado para publicarse sin incluir documentos reales. Los
PDFs, hojas Excel operativas, reportes generados, ZIPs, caches y archivos del
sistema estan ignorados en `.gitignore`.

No publiques archivos reales de trabajo junto con el codigo. Si necesitas probar
el script, usa datos propios en local o fixtures ficticios.

## Requisitos

- Python 3.10 o superior.
- Tesseract OCR instalado y disponible en `PATH`, o indicado con `--tesseract`.
- Dependencias de Python listadas en `requirements.txt`.

En Windows, la ruta habitual de Tesseract es:

```text
C:\Program Files\Tesseract-OCR\tesseract.exe
```

## Instalacion

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

En Windows:

```bat
py -3 -m venv .venv
.venv\Scripts\activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Uso

Ejecuta primero una simulacion para generar el reporte sin renombrar archivos:

```bash
python renombrar_actas_ocr.py --dry-run --excel "ruta/al/archivo.xlsx"
```

Si el reporte es correcto, ejecuta el renombrado:

```bash
python renombrar_actas_ocr.py --excel "ruta/al/archivo.xlsx"
```

Para procesar una carpeta especifica:

```bash
python renombrar_actas_ocr.py "ruta/a/la/carpeta" --excel "ruta/al/archivo.xlsx" --dry-run
```

Si Tesseract no esta en `PATH`:

```bash
python renombrar_actas_ocr.py --tesseract "C:\Program Files\Tesseract-OCR\tesseract.exe" --dry-run
```

Tambien puedes definir la ruta en una variable de entorno:

```bash
export TESSERACT_CMD="/ruta/a/tesseract"
```

Para instrucciones paso a paso en Windows, consulta
[`GUIA_USO_WINDOWS.md`](GUIA_USO_WINDOWS.md).

## Formato del Excel

El script lee el Excel por posicion de columnas:

| Columna | Uso |
| --- | --- |
| B | Nombre de quien entrega |
| E | Fecha de baja |
| F | Nombre final del PDF |

El archivo puede tener mas columnas, pero debe conservar esas posiciones.

Hay un archivo ficticio en `examples/datos_ejemplo.xlsx` para mostrar la
estructura esperada sin exponer datos reales.

## Reporte

Al terminar, el programa genera `reporte_renombrado_actas.xlsx` por defecto. El
reporte incluye:

- archivo original;
- fecha detectada;
- nombre detectado;
- nombre encontrado en Excel;
- nuevo nombre;
- score de coincidencia;
- estado del procesamiento.

Puedes cambiar la ruta del reporte:

```bash
python renombrar_actas_ocr.py --reporte "salida/reporte.xlsx" --dry-run
```

## Validacion antes de publicar

Antes de subir este proyecto a GitHub:

```bash
git status --short
python3 -m compileall -q renombrar_actas_ocr.py
python3 renombrar_actas_ocr.py --help
```

Revisa que no aparezcan PDFs, Excel reales, reportes, ZIPs, `.env`, caches o
archivos del sistema en el area de cambios.
