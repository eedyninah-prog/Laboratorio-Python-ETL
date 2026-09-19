# Laboratorio-Python-ETL
Tarea de clases
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "source": [
        "#Laboratorio 2\n",
        "##Parte 1\n",
        "### Ingesta y Limpieza de Cadenas (Strings)"
      ],
      "metadata": {
        "id": "cuy7uubM_AUs"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# Cadena cruda tal como llega del sistema legacy, con espacios y símbolos $$ sobrantes\n",
        "log_crudo = \" $$[ESTADO:ok]||trx_id:9934||monto:S/.1250.50||alerta:falso$$ \""
      ],
      "metadata": {
        "id": "7sGpIIV2-_VH"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "collapsed": true,
        "id": "8BYiuZEY83w9"
      },
      "outputs": [],
      "execution_count": null,
      "source": [
        "# strip(\"$ \") elimina de los extremos cualquier combinación de espacio y '$'\n",
        "# Guardamos el resultado en una nueva variable para no perder el paso\n",
        "log_limpio = log_crudo.strip(\"$ \")\n"
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# Convertimos toda la cadena limpia a mayúsculas con upper()\n",
        "log_mayuscula = log_limpio.upper()\n",
        "log_mayuscula"
      ],
      "metadata": {
        "id": "BDVkx-Y6JEli"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# split(\"||\") divide la cadena en cada aparición de '||', devolviendo una lista de campos\n",
        "lista_campos = log_mayuscula.split(\"||\")\n",
        "\n",
        "# Imprimimos el resultado como pide la consigna\n",
        "print(lista_campos)"
      ],
      "metadata": {
        "id": "kvgBjNoEPOHY"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "#Parte 2\n",
        "###Colecciones y Extracción Analítica"
      ],
      "metadata": {
        "id": "RHV6zIT0SE2c"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# El monto está en la posición 2 de la lista (índice empieza en 0: 0=estado, 1=trx_id, 2=monto, 3=alerta)\n",
        "campo_monto = lista_campos[2]\n",
        "campo_monto"
      ],
      "metadata": {
        "id": "yu2VeqjDSJOz"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# \"MONTO:S/.\" ocupa las primeras 9 posiciones (índices 0 a 8), por eso rebanamos desde el índice 9 hasta el final\n",
        "campo_monto2 = campo_monto[9:]\n",
        "\n",
        "# float() convierte el texto numérico extraído a un número decimal, para poder compararlo después\n",
        "monto_transaccion = float(campo_monto2)\n",
        "\n",
        "monto_transaccion\n"
      ],
      "metadata": {
        "id": "5BE4SDQGYYf8"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "##Parte3\n",
        "###Lógica Condicional y Tuplas de Control"
      ],
      "metadata": {
        "id": "q8X2C_ZYcWgh"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# Tupla con los umbrales de auditoría: posición 0 = límite de revisión manual, posición 1 = límite de bloqueo\n",
        "# Se usa tupla porque estos valores no deben modificarse durante la ejecución\n",
        "umbrales_auditoria = (500.0, 2000.0)\n",
        "\n",
        "# Si supera el límite de bloqueo (posición 1), se bloquea la transacción\n",
        "if monto_transaccion > umbrales_auditoria[1]:\n",
        "  print(\"ALERTA ROJA: Transacción bloqueada. Supera los $2000.\")\n",
        "\n",
        "# Si no llegó al bloqueo pero superó el límite de revisión (posición 0), va a revisión manual\n",
        "# (no hace falta comparar <= 2000 explícitamente: si llegó hasta aquí es porque el if anterior ya fue falso)\n",
        "elif monto_transaccion > umbrales_auditoria[0]:\n",
        "  print(\"ALERTA AMARILLA: Transacción enviada a revisión manual.\")\n",
        "\n",
        "# Si no cumplió ninguna condición anterior, se procesa automáticamente\n",
        "else:\n",
        "  print(\"VERDE: Transacción procesada automáticamente.\")\n"
      ],
      "metadata": {
        "id": "9_H3NOeUcZGH"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "##Parte 4\n",
        "###Diccionarios y Automatización con Bucles"
      ],
      "metadata": {
        "id": "BVpuLx_zxcAs"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# Diccionario que consolida la información del reporte diario:\n",
        "# fecha (texto), lotes_procesados (lista de montos) y operador (texto)\n",
        "reporte_diario = {\n",
        "    \"fecha\": \"2026-06-07\",\n",
        "    \"lotes_procesados\": [350.0, 1250.50, 90.0, 2100.0],\n",
        "    \"operador\": \"edy enrique nina huiilca\"\n",
        "}\n",
        "\n",
        "# enumerate(..., 1) recorre la lista dentro del diccionario entregando índice y valor a la vez,\n",
        "# empezando el índice en 1 (no en 0) como pide la consigna\n",
        "for indice, valor in enumerate(reporte_diario[\"lotes_procesados\"], 1):\n",
        "    print(f\"Lote N° {indice} procesado por un valor de ${valor}\")\n",
        "\n",
        "# Simulación de conexión inestable: el bucle while se repite mientras intento sea <= 3\n",
        "intento = 1\n",
        "while intento <= 3:\n",
        "    print(f\"Intentando conectar al servidor de destino... Intento {intento}\")\n",
        "    # Incrementamos el contador en cada vuelta; sin esto el bucle sería infinito\n",
        "    intento += 1"
      ],
      "metadata": {
        "id": "ITjpzG13xeUz"
      },
      "execution_count": null,
      "outputs": []
    }
  ]
}
