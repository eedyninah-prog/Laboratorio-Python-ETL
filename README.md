# Laboratorio 2: Python para Análisis de Datos (ETL)

## Descripción
Laboratorio práctico del curso "Python para Análisis de Datos (ETL)" 
(Instructor: Christian Condori). Simula un caso de negocio de una 
pasarela de pagos: limpieza de un registro de transacción en texto 
crudo, evaluación de su nivel de riesgo y consolidación en un reporte 
estructurado — todo con Python nativo, sin librerías externas.

## Conceptos aplicados
- Manipulación de strings: `strip()`, `upper()`, `split()`, slicing
- Conversión de tipos: `float()`
- Tuplas (para valores de configuración inmutables)
- Estructuras condicionales: `if / elif / else`
- Diccionarios y listas anidadas
- Bucles `for` con `enumerate()` y bucles `while`

## Estructura del laboratorio
1. **Ingesta y limpieza de cadenas**: limpieza de espacios/símbolos y separación de campos
2. **Extracción analítica**: aislamiento y conversión del monto de la transacción
3. **Lógica condicional**: clasificación de riesgo según umbrales de auditoría (tupla)
4. **Diccionarios y bucles**: consolidación en un reporte diario y simulación de reintentos de conexión

## Herramientas
Google Colab, Python 3 (sin Pandas)
