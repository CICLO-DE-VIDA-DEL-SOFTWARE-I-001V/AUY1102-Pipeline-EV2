# Análisis de Pruebas Unitarias y Calidad de Software (Indicador IE3.1.1)
**Responsable:** Isidro Benítez  
**Asignatura:** Ciclo de Vida del Software I (AUY1102)  
**Evaluación:** Evaluación Parcial N° 2  
**Entorno de Ejecución:** Jest / ts-jest (TypeScript)

---

## 1. Marco Teórico: Conceptos Clave de Calidad de Software

Para dar cumplimiento al indicador **IE3.1.1** y responder a la transformación digital de la empresa *"Integration Solutions"*, se analiza el impacto de cada concepto en el ciclo de vida del software:

### 1.1 Pruebas Unitarias (Unit Testing)
* **Definición:** Verificación individual e independiente de la unidad lógica más pequeña de software (funciones, clases, Value Objects) sin interactuar con bases de datos ni servicios externos.
* **Aporte a la Calidad:** Permiten la detección inmediata de errores de lógica en fase temprana (*Shift-Left Testing*), reduciendo costos operacionales y garantizando que cada componente base sea predecible y seguro.

### 1.2 Calidad del Código Estático vs. Dinámico
* **Código Estático (SAST):** Análisis del código fuente en reposo (sin ejecutarlo) en busca de vulnerabilidades, reglas de formato, *bad smells* y complejidad ciclomática.
* **Código Dinámico (DAST):** Evaluación de la aplicación en tiempo de ejecución para monitorear el consumo de memoria, tiempos de respuesta y estabilidad ante datos no controlados.

### 1.3 Análisis de Seguridad y Vulnerabilidades
* **Definición:** Inspección automatizada para detectar fallas tanto en el código propio como en dependencias externas (SCA).
* **Aporte a la Calidad:** Previene brechas de seguridad críticas (ej. inyecciones, fugas de credenciales) antes de que el software sea desplegado a producción.

### 1.4 Pruebas Integradas (Integration Testing)
* **Definición:** Validación de la interacción fluida entre múltiples módulos, contratos de API o capas de servicio.
* **Aporte a la Calidad:** Aseguran que los módulos probados unitariamente sigan funcionando correctamente al acoplarse entre sí.

### 1.5 Pruebas de Estrés (Stress Testing)
* **Definición:** Sometimiento de la aplicación a niveles de carga y concurrencia por encima del umbral operativo normal.
* **Aporte a la Calidad:** Determinan el comportamiento del software al saturarse, su capacidad de recuperación y evitan la caída no controlada de los servicios.

### 1.6 Pruebas de Regresión (Regression Testing)
* **Definición:** Ejecución automatizada de la suite de pruebas tras cada cambio, parche o actualización en el repositorio.
* **Aporte a la Calidad:** Garantizan que la corrección de un bug o la adición de una nueva característica no rompa funcionalidades previas ya aprobadas.

---

## 2. Resultados de la Ejecución Práctica en Consola

### 2.1 Comando de Ejecución Configurado
En el archivo `package.json` se configuró el script:
```bash
npm run test:coverage
```
El cual ejecuta internamente: `jest --coverage --verbose`.

### 2.2 Resumen de Ejecución de Pruebas
* **Suites de Pruebas:** 10 suites totales / 10 aprobadas (100% de éxito).
* **Pruebas Unitarias:** 18 pruebas ejecutadas / 18 aprobadas / 0 fallidas.
* **Tiempo de Ejecución:** 6.438 segundos.
* **Diagnóstico de Fallos:** Las pruebas existentes superan sus aserciones funcionales sin registrar fallos. El problema crítico detectado radica en el déficit de cobertura de código en módulos clave.

---

## 3. Matriz de Cobertura de Código Obtenida

| Archivo / Módulo | % Sentencias (Stmts) | % Ramas (Branch) | % Funciones (Funcs) | % Líneas (Lines) | Líneas No Cubiertas (`Uncovered Line #s`) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Global (`All files`)** | **43.28%** | **60.00%** | **56.66%** | **43.28%** | N/A |
| **`src/index.ts`** | 62.50% | 100.00% | 25.00% | 62.50% | Líneas **8-10** |
| **`src/quality/errores-object.ts`** | 0.00% | 100.00% | 0.00% | 0.00% | Líneas **2-6** |
| **`src/quality/insecure-1-object.ts`** | 0.00% | 100.00% | 100.00% | 0.00% | Líneas **1-10** |
| **`src/quality/insecure-2-object.ts`** | 0.00% | 100.00% | 0.00% | 0.00% | Líneas **1-32** |
| **`src/value-object/index.ts`** | 100.00% | 100.00% | 100.00% | 100.00% | Ninguna (100% cubierto) |
| **`src/value-object/primitives/date-value-object.ts`** | 6.66% | 0.00% | 0.00% | 6.66% | Líneas **3-35** |
| **`src/value-object/primitives/enum-value-object.ts`** | 83.33% | 0.00% | 100.00% | 83.33% | Línea **11** |
| **`src/value-object/primitives/index.ts`** | 100.00% | 100.00% | 100.00% | 100.00% | Ninguna (100% cubierto) |
| **`src/value-object/primitives/number-value-object.ts`** | 100.00% | 100.00% | 100.00% | 100.00% | Ninguna (100% cubierto) |
| **`src/value-object/primitives/string-value-object.ts`** | 100.00% | 100.00% | 100.00% | 100.00% | Ninguna (100% cubierto) |

---

## 4. Análisis Crítico del Código No Cubierto

1. **Módulo de Calidad y Seguridad (`src/quality/`):**
   * Los archivos `errores-object.ts`, `insecure-1-object.ts` e `insecure-2-object.ts` presentan un **0% de cobertura total**. 
   * **Hallazgo:** Este módulo no cuenta con ninguna prueba unitaria escrita en `test/`. Al inspeccionar sus nombres y líneas, representan componentes con fallas de calidad y vulnerabilidades destinadas a ser analizadas por **SonarCloud** y **Snyk** en la Parte 2.
2. **Módulo de Primitivas (`date-value-object.ts`):**
   * Únicamente se cubre el 6.66% del archivo; las líneas **3 a 35** no son ejecutadas por los tests actuales, lo que deja sin validar la lógica de validación de fechas y conversiones.
3. **Módulo de Enumeraciones (`enum-value-object.ts`):**
   * La **línea 11** no está cubierta, correspondiente al camino alternativo (`else` o lanzamiento de excepción) cuando se recibe un valor inválido fuera del enum.