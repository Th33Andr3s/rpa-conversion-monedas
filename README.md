# CurrencyConverter — UiPath RPA Project

> **Evidencia:** AA2-EV01 | Programa: Automatización de Procesos para la Eficiencia Organizacional
> **Versión:** 1.0 | **UiPath Studio:** 26.0.189.0 STS | **Framework:** Windows (.NET 6)

---

## ¿Qué hace este proyecto?

Automatiza la conversión de monedas consultando tasas de cambio en **tiempo real** desde la API pública
`open.er-api.com`. Permite realizar múltiples conversiones en una sola ejecución con una interfaz de
diálogos simple y validación completa de entradas.

---

## Estructura del Proyecto

```
CurrencyConverter/
├── Main.xaml                        # Flujo principal del robot
├── project.json                     # Configuración del proyecto UiPath
├── Workflows/
│   └── GetConversionRate.xaml       # Workflow reutilizable de conversión
├── Config/
│   └── config.json                  # Configuración y referencia de monedas
├── Tests/
│   └── TestCases.md                 # Plan de pruebas documentado
└── README.md                        # Este archivo
```

---

## Requisitos Previos

| Requisito                | Versión / Detalle                        |
| ------------------------ | ---------------------------------------- |
| UiPath Studio            | 26.0.189.0 STS o superior                |
| UiPath.System.Activities | 24.10.x o superior (se instala auto)     |
| Conexión a Internet      | Requerida para consultar la API          |
| Sistema Operativo        | Windows 10 / 11 (framework Windows/NET6) |
| API Key                  | No requerida — API gratuita              |

> **Nota:** UiPath.UIAutomation.Activities también se instala automáticamente al abrir el proyecto.

---

## Cómo Importar el Proyecto

### Opción A — Desde UiPath Studio

1. Descomprimir en una carpeta de trabajo
2. Abrir **UiPath Studio 26.0**
3. Ir a **File → Open** (o `Ctrl+O`)
4. Navegar a la carpeta descomprimida y seleccionar `project.json`
5. UiPath solicitará instalar las dependencias automáticamente → hacer clic en **Restore**
6. Esperar a que se instalen los paquetes desde NuGet

### Opción B — Desde el panel de inicio

1. Descomprimir el `.zip`
2. En UiPath Studio, en la pantalla de inicio hacer clic en **Open Project**
3. Seleccionar `project.json`

---

## Cómo Ejecutar la Automatización

### Ejecución desde Studio (Desarrollo / Pruebas)

1. Con el proyecto abierto, verificar que `Main.xaml` está seleccionado en el panel de Project
2. Presionar **F5** o hacer clic en **Run** (▶) en la barra de herramientas
3. También puede hacer clic derecho sobre `Main.xaml` → **Set As Main** → **Run**

### Flujo de Ejecución (Paso a Paso)

```
┌─────────────────────────────────────────────────────────────────┐
│                    INICIO DEL PROCESO                           │
├─────────────────────────────────────────────────────────────────┤
│  1. MessageBox de Bienvenida                                    │
│                      ↓                                          │
│  ┌─────────────── LOOP (mientras el usuario desee) ──────────┐  │
│  │                                                           │  │
│  │  2. InputDialog → Moneda ORIGEN  (ej: USD)               │  │
│  │  3. InputDialog → Moneda DESTINO (ej: COP)               │  │
│  │  4. InputDialog → MONTO          (ej: 1000)              │  │
│  │             ↓ (validaciones automáticas)                  │  │
│  │  5. HTTP GET → open.er-api.com/v6/latest/USD             │  │
│  │             ↓                                             │  │
│  │  6. Parseo JSON → extraer tasa de cambio                 │  │
│  │  7. Cálculo    → monto * tasa                            │  │
│  │             ↓                                             │  │
│  │  8. MessageBox → Resultado con formato completo          │  │
│  │  9. MessageBox → ¿Otra conversión? [Sí / No]            │  │
│  └──────────────── Si NO → salir del loop ─────────────────┘  │
│                      ↓                                          │
│  10. MessageBox de Despedida                                    │
├─────────────────────────────────────────────────────────────────┤
│                       FIN                                       │
└─────────────────────────────────────────────────────────────────┘
```

---

## API Utilizada

| Parámetro     | Valor                                        |
| ------------- | -------------------------------------------- |
| Nombre        | ExchangeRate-API (endpoint gratuito)         |
| URL           | `https://open.er-api.com/v6/latest/{MONEDA}` |
| Autenticación | Sin API Key (plan free, 1,500 req/mes)       |
| Formato       | JSON                                         |
| Actualización | Cada 24 horas                                |
| Monedas       | +160 divisas ISO 4217                        |

**Ejemplo de respuesta:**

```json
{
  "result": "success",
  "base_code": "USD",
  "rates": {
    "USD": 1.0,
    "EUR": 0.9202,
    "COP": 4080.36,
    "GBP": 0.7895
  }
}
```

---

## Monedas Soportadas (más comunes)

| Código | Moneda               | Código | Moneda           |
| ------ | -------------------- | ------ | ---------------- |
| USD    | Dólar estadounidense | MXN    | Peso mexicano    |
| EUR    | Euro                 | JPY    | Yen japonés      |
| COP    | Peso colombiano      | CAD    | Dólar canadiense |
| GBP    | Libra esterlina      | CHF    | Franco suizo     |
| BRL    | Real brasileño       | ARS    | Peso argentino   |
| PEN    | Sol peruano          | CLP    | Peso chileno     |

> Lista completa: https://www.exchangerate-api.com/docs/supported-currencies

---

## Posibles Errores y Soluciones

| Error / Síntoma                            | Causa                   | Solución                                             |
| ------------------------------------------ | ----------------------- | ---------------------------------------------------- |
| "La moneda de origen no puede estar vacía" | Input vacío             | Ingresar código válido (ej: USD)                     |
| "Moneda 'XYZ' no encontrada en la API"     | Código inválido         | Usar código ISO 4217 (ver tabla arriba)              |
| "Input string was not in a correct format" | Monto no numérico       | Usar solo dígitos y punto decimal (ej: 1500.50)      |
| "El monto debe ser mayor que cero"         | Monto ≤ 0               | Ingresar un valor positivo                           |
| WebException / "Unable to connect"         | Sin conexión a internet | Verificar conexión activa y acceso a open.er-api.com |
| Error al restaurar dependencias            | Problema NuGet          | Ir a **Manage Packages** y reinstalar manualmente    |
| Project.json no abre en Studio             | Versión incompatible    | Verificar UiPath Studio ≥ 26.0 con Windows framework |

---

## Arquitectura del Proyecto

```
Main.xaml
├── MÓDULO 1: Captura y validación de entradas
│   ├── InputDialog × 3 (origen, destino, monto)
│   └── Validaciones: vacío, numérico, positivo
│
├── MÓDULO 2: Consulta API (System.Net.WebClient)
│   └── HTTP GET → open.er-api.com
│
├── MÓDULO 3: Parseo JSON y cálculo
│   ├── Newtonsoft.Json.Linq.JObject.Parse()
│   ├── Verificación de moneda existente
│   └── Math.Round(amount × rate, 4)
│
└── MÓDULO 4: Presentación de resultados
    └── MessageBox formateado

Workflows/GetConversionRate.xaml
└── Workflow reutilizable con argumentos IN/OUT
    (Demostra separación de responsabilidades)
```

---
