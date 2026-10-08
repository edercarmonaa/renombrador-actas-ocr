# Guia de uso en Windows

Esta guia explica como ejecutar `renombrar_actas_ocr.py` en Windows para
renombrar PDFs escaneados de actas de entrega usando OCR y un Excel de control.

## Objetivo

El programa revisa los PDFs y solo procesa los documentos cuya primera hoja
corresponde a:

```text
ACTA DE ENTREGA DE TERMINO DE OBRA
```

Cuando el PDF es de ese tipo, extrae por OCR la fecha del acta y el nombre de
quien entrega, compara esos datos contra el Excel y, si coinciden, renombra
cada PDF con el valor de la columna F.

## Archivos necesarios

En la misma carpeta debes tener:

- `renombrar_actas_ocr.py`
- `requirements.txt`
- Los PDFs a revisar
- El Excel con las claves para renombrar

El Excel debe conservar estas columnas:

| Columna | Uso |
| --- | --- |
| B | Nombre de quien entrega |
| E | Fecha de baja |
| F | Nombre nuevo para el PDF |

## Instalar Python en Windows

1. Descarga Python desde <https://www.python.org/downloads/windows/>.
2. Al instalarlo, marca la opcion `Add Python to PATH`.
3. Verifica la instalacion abriendo CMD y ejecutando:

```bat
py --version
```

## Instalar Tesseract OCR en Windows

El programa necesita Tesseract porque los PDFs son escaneados.

1. Instala Tesseract OCR para Windows.
2. Si el instalador permite agregarlo al `PATH`, activa esa opcion.
3. La ruta comun de instalacion es:

```text
C:\Program Files\Tesseract-OCR\tesseract.exe
```

Para verificarlo, abre CMD y ejecuta:

```bat
tesseract --version
```

Si Windows no reconoce el comando, puedes indicar la ruta manualmente con
`--tesseract`.

## Preparar el programa

Abre CMD en la carpeta donde esta `renombrar_actas_ocr.py`.

Puedes hacerlo asi:

1. Abre la carpeta en el Explorador de archivos.
2. Escribe `cmd` en la barra de direccion.
3. Presiona Enter.

Luego ejecuta:

```bat
py -3 -m venv .venv
.venv\Scripts\activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Probar sin renombrar

Siempre ejecuta primero una simulacion:

```bat
python renombrar_actas_ocr.py --dry-run
```

Esto genera un reporte llamado:

```text
reporte_renombrado_actas.xlsx
```

Revisa ese reporte antes de renombrar.

## Renombrar los PDFs

Si el reporte se ve correcto, ejecuta:

```bat
python renombrar_actas_ocr.py
```

## Usar un Excel especifico

Si hay mas de un Excel en la carpeta, indica cual usar:

```bat
python renombrar_actas_ocr.py --excel "archivo_de_control.xlsx"
```

## Indicar ruta de Tesseract

Si Tesseract no esta en el `PATH`, ejecuta:

```bat
python renombrar_actas_ocr.py --tesseract "C:\Program Files\Tesseract-OCR\tesseract.exe"
```

Tambien puedes combinarlo con simulacion:

```bat
python renombrar_actas_ocr.py --dry-run --tesseract "C:\Program Files\Tesseract-OCR\tesseract.exe"
```

## Procesar otra carpeta

Puedes indicar una carpeta diferente:

```bat
python renombrar_actas_ocr.py "C:\ruta\a\carpeta"
```

Tambien puedes combinar opciones:

```bat
python renombrar_actas_ocr.py "C:\ruta\a\carpeta" --excel "C:\ruta\a\archivo.xlsx" --dry-run
```

## Errores comunes

### No encontre tesseract

- Verifica que Tesseract OCR este instalado.
- Usa la opcion `--tesseract` con la ruta completa.

### Hay varios Excel

- Usa `--excel` para indicar el archivo correcto.

### destino ya existe

- Ya existe un PDF con el nombre final.
- El programa no lo reemplaza para evitar perdida de archivos.

## Notas

- Esta guia esta pensada solo para Windows.
- Siempre usa `--dry-run` primero.
- Los PDFs que no sean `ACTA DE ENTREGA DE TERMINO DE OBRA` se omiten y quedan
  marcados en el reporte.
- El programa no reemplaza un PDF si el nombre destino ya existe.
- Al terminar genera un reporte con fecha extraida, nombre extraido,
  coincidencia del Excel, nuevo nombre y estado.
