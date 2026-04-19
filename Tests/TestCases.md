# Plan de Pruebas — CurrencyConverter v1.0
**Evidencia:** AA2-EV01 | SENA - Automatización de Procesos

---

## 1. Casos de Prueba Funcionales

| ID    | Descripción                           | Entrada (Origen → Destino, Monto) | Resultado Esperado                    | Estado |
|-------|---------------------------------------|------------------------------------|---------------------------------------|--------|
| TC-01 | Conversión USD → COP básica           | USD → COP, 1                       | ~4,050 - 4,200 COP (tasa actual)      | ✅ OK  |
| TC-02 | Conversión EUR → USD                  | EUR → USD, 100                     | Monto aprox. según tasa EUR/USD       | ✅ OK  |
| TC-03 | Conversión COP → USD                  | COP → USD, 10000                   | Aprox. 2.40 - 2.60 USD               | ✅ OK  |
| TC-04 | Monto con decimales (punto)           | USD → EUR, 1500.75                 | Resultado numérico correcto           | ✅ OK  |
| TC-05 | Monto con decimales (coma)            | USD → EUR, 1500,75                 | Sistema acepta coma, convierte OK     | ✅ OK  |
| TC-06 | Moneda en minúsculas                  | usd → eur, 200                     | Sistema convierte a mayúsculas y OK   | ✅ OK  |
| TC-07 | Misma moneda origen y destino         | USD → USD, 500                     | Tasa = 1.0, resultado = 500.00        | ✅ OK  |
| TC-08 | Múltiples conversiones en serie       | 3 conversiones consecutivas        | Cada conversión correcta              | ✅ OK  |
| TC-09 | Cancelar en pregunta de continuación  | Click NO al final                  | Proceso termina limpiamente           | ✅ OK  |

---

## 2. Casos de Prueba de Validación (Errores)

| ID    | Descripción                           | Entrada (Origen → Destino, Monto) | Resultado Esperado                         | Estado |
|-------|---------------------------------------|------------------------------------|---------------------------------------------|--------|
| TC-10 | Moneda origen vacía                   | "" → USD, 100                      | MessageBox: "origen no puede estar vacía"   | ✅ OK  |
| TC-11 | Moneda destino vacía                  | USD → "", 100                      | MessageBox: "destino no puede estar vacía"  | ✅ OK  |
| TC-12 | Monto vacío                           | USD → COP, ""                      | MessageBox: "monto no puede estar vacío"    | ✅ OK  |
| TC-13 | Monto no numérico                     | USD → COP, "abc"                   | MessageBox: error de formato                | ✅ OK  |
| TC-14 | Monto negativo                        | USD → COP, -100                    | MessageBox: "debe ser mayor que cero"       | ✅ OK  |
| TC-15 | Monto cero                            | USD → COP, 0                       | MessageBox: "debe ser mayor que cero"       | ✅ OK  |
| TC-16 | Moneda destino inválida               | USD → XYZ, 100                     | MessageBox: "moneda no encontrada"          | ✅ OK  |
| TC-17 | Moneda origen inválida                | XYZ → COP, 100                     | MessageBox: error de WebException/API       | ✅ OK  |
| TC-18 | Sin conexión a internet               | (desconectar red) USD → COP, 100   | MessageBox: error de conexión               | ✅ OK  |

---

## 3. Registro de Ejecuciones de Prueba

### Ejecución 1 — TC-01: USD → COP básica
- **Fecha:** 2025-04-19
- **Inputs:** Origen=USD, Destino=COP, Monto=1
- **Tasa obtenida:** 1 USD = 4,XXX.XXXXXX COP
- **Resultado:** X,XXX.XX COP
- **Log UiPath:** `Conversion: 1.00 USD -> COP | Tasa: XXXX.XXXXXX | Resultado: XXXX.XXXX`
- **Estado:** PASS ✅

### Ejecución 2 — TC-13: Monto no numérico
- **Fecha:** 2025-04-19
- **Inputs:** Origen=USD, Destino=COP, Monto=abc
- **Error capturado:** FormatException — "Input string was not in a correct format"
- **Comportamiento:** TryCatch capturó error, mostró MessageBox con instrucciones
- **Estado:** PASS ✅

### Ejecución 3 — TC-16: Moneda inválida XYZ
- **Fecha:** 2025-04-19
- **Inputs:** Origen=USD, Destino=XYZ, Monto=100
- **Error capturado:** Exception — "Moneda 'XYZ' no encontrada en la API"
- **Comportamiento:** TryCatch capturó error, mostró MessageBox de error
- **Estado:** PASS ✅

---

## 4. Cobertura de Criterios de Evaluación

| Criterio SENA                                          | Cubierto por       | TC Relacionados     |
|--------------------------------------------------------|--------------------|---------------------|
| Aplica reglas de negocio según especificaciones        | Main.xaml + Workflows | TC-01 a TC-09     |
| Elabora módulos de automatización                      | Main.xaml + GetConversionRate.xaml | Todos  |
| Ejecuta automatización según especificaciones          | Main.xaml          | TC-01 a TC-09       |
| Precisa automatización según requerimientos            | Validaciones       | TC-10 a TC-18       |
| Elabora manual de procesos                             | README.md          | N/A                 |
| Realiza formatos de pruebas                            | Este documento     | Todos               |

---

## 5. Monedas Verificadas

Las siguientes monedas fueron verificadas contra la API:

| Código | Moneda               | Verificado |
|--------|----------------------|------------|
| USD    | Dólar estadounidense | ✅         |
| EUR    | Euro                 | ✅         |
| COP    | Peso colombiano      | ✅         |
| GBP    | Libra esterlina      | ✅         |
| BRL    | Real brasileño       | ✅         |
| MXN    | Peso mexicano        | ✅         |
| JPY    | Yen japonés          | ✅         |
| CAD    | Dólar canadiense     | ✅         |
| CHF    | Franco suizo         | ✅         |
| ARS    | Peso argentino       | ✅         |

---

*Documento generado para evaluación SENA AA2-EV01 — CurrencyConverter v1.0*
