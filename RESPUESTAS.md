# Respuestas del Parcial 2

## Fallas corregidas en `Jenkinsfile`

1. `Build image` estaba antes de `Regresion`; la imagen se construía aunque la regresión fallara, por lo que las pruebas no actuaban como puerta.
2. El comando de Bruno terminaba con `|| true`; ocultaba su código de error y permitía que el pipeline siguiera en verde con pruebas fallidas.
3. `junit` buscaba `bruno/smoke/results.xml`; no encontraba el reporte generado en `bruno/regresion/results.xml` y no registraba los resultados de la regresión.

## Secuencia de builds

| Build | Resultado | SHA ejecutado | Cambio |
| --- | --- | --- | --- |
| #1 | Verde | `1d9bd598033c983d856667e5cbd02978c3f585a8` | `Jenkinsfile` corregido, antes de agregar los casos del tope. |
| #2 | Rojo | `63e0610406692835a91104c410c7a5bad3d8185c` | Casos de Bruno que detectan el cobro superior a Q60. |
| #3 | Verde | `cfb52189230abcdc959c36180bdcdc965fb08d96` | Incluye la corrección de `api.py` del commit anterior `3d2ea53b343302da063fad9d9ebc4705219115be`; el commit ejecutado agregó evidencia. |

Aserción fallida en el Test Result del build #2: `tope Q60 despues de siete horas` (`16-cobro-sobre-tope`).

## Aprendizajes aplicados

En la hoja de trabajo de Jenkins con Docker aprendí a preparar el contenedor con las herramientas que necesita el pipeline y acceso al motor de Docker para construir la imagen. De Bruno Books retomé las pruebas de valores frontera y los reportes JUnit: aquí comprobé el cobro justo en Q60 y después de superar ese límite. El principio de responsabilidad única de SOLID me ayudó a mantener una tarea clara por etapa del pipeline: iniciar el API, ejecutar la regresión y construir la imagen. Al quitar `|| true` y publicar el reporte de la colección correcta, aprendí que una prueba solo sirve como puerta de calidad si su falla detiene el build y queda registrada.
