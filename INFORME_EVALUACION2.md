# INFORME TÉCNICO DE EVALUACIÓN PARCIAL N° 2
## Aseguramiento de la Calidad del Software: Pruebas Unitarias y Análisis de Seguridad Estático

* **Asignatura:** Ciclo de Vida del Software I (AUY1102)
* **Caso de Estudio:** Transformación Digital "Integration Solutions"
* **Fecha:** Noviembre 2026
* **Equipo de Trabajo (Roles Asignados):**
  * **Isidro Benítez** — Responsable Parte 1: Pruebas Unitarias y Cobertura de Código (QA Lead)
  * **Rafael Heneche** — Responsable Parte 2: Análisis Estático de Seguridad y Vulnerabilidades (SecOps Lead)

---

## 1. CONTROL DE REPOSITORIO Y ACCESOS

* **Repositorio Original (Upstream):** `https://github.com/Fundacion-Instituto-Profesional-Duoc-UC/AUY1102-Pipeline`
* **Repositorio Fork de Trabajo:** `https://github.com/<ORGANIZACION_O_USUARIO>/AUY1102-Pipeline-EV2`
* **Enlace Directo al Archivo Markdown:** `https://github.com/<ORGANIZACION_O_USUARIO>/AUY1102-Pipeline-EV2/blob/main/INFORME_EVALUACION2.md`

---

## 2. PARTE 1: ANÁLISIS DE CÓDIGO MEDIANTE PRUEBAS UNITARIAS (ISIDRO BENÍTEZ)

### 2.1 Marco Conceptual y Análisis Teórico (Indicador IE3.1.1 - 50%)
Para responder a los objetivos de modernización de *Integration Solutions*, se analizan los conceptos fundamentales de aseguramiento de calidad y su contribución directa al producto de software:

1. **Pruebas Unitarias (Unit Testing):**
   * *Definición:* Validación aislada de la unidad ejecutable más pequeña del sistema (funciones, métodos o Value Objects), aislando dependencias externas mediante dobles de prueba (*mocks/stubs*).
   * *Contribución a la calidad:* Permiten la detección inmediata de errores de lógica funcional durante la codificación local (*Shift-Left Testing*), reduciendo drásticamente el costo de corrección frente a fallos encontrados en producción.
2. **Calidad de Código Estático vs. Dinámico:**
   * *Código Estático:* Evaluación del código fuente en reposo sin ser ejecutado. Detecta transgresión de estilos, patrones antipatrón (*bad smells*), complejidad ciclomática excesiva y vulnerabilidades sintácticas.
   * *Código Dinámico:* Evaluación del software en tiempo de ejecución. Permite monitorear consumo de memoria RAM, saturación de CPU, fugas de memoria y tiempos de respuesta.
3. **Análisis de Seguridad y Vulnerabilidades del Código:**
   * *Definición:* Inspección automatizada para identificar defectos que puedan ser explotados por atacantes, abarcando tanto el código fuente propio (SAST) como las dependencias de terceros (SCA).
   * *Contribución a la calidad:* Evita la exposición de datos sensibles, accesos no autorizados y fallos del estándar OWASP Top 10 antes del despliegue.
4. **Pruebas Integradas (Integration Testing):**
   * *Definición:* Verificación de la interacción y flujo de datos entre dos o más módulos autónomos, subsistemas o servicios externos (APIs, bases de datos).
   * *Contribución a la calidad:* Aseguran que los contratos de interfaz y la comunicación intermodular sean consistentes.
5. **Pruebas de Estrés (Stress Testing):**
   * *Definición:* Sometimiento del sistema a cargas transaccionales y de concurrencia que superan ampliamente los límites normales de diseño.
   * *Contribución a la calidad:* Identifican el punto de quiebre de la infraestructura, garantizan una degradación elegante del servicio y verifican la resiliencia en la recuperación del sistema.
6. **Pruebas de Regresión (Regression Testing):**
   * *Definición:* Reejecución sistemática de las baterías de pruebas existentes tras la incorporación de nuevas características o parches de bugs.
   * *Contribución a la calidad:* Certifican que los cambios introducidos no deterioren funcionalidades previas que ya operaban correctamente.

---

### 2.2 Implementación Práctica: Configuración y Script de Pruebas
Para la ejecución de pruebas con reporte de cobertura detallado, Isidro Benítez configuró en el archivo `package.json` el script:
```json
"scripts": {
  "test:coverage": "jest --coverage --verbose"
}
```
**Comando ejecutado:**
```bash
npm run test:coverage
```

### 2.3 Evidencias y Resultados de Ejecución
* **Suites de Pruebas:** 10 suites totales / 10 aprobadas.
* **Pruebas Unitarias Ejecutadas:** 18 pruebas / 18 pasadas / 0 fallidas.
* **Tiempo Total:** 6.438 segundos.

```text
 PASS  test/value-object/primitives/string-value-object.test.ts
 PASS  test/value-object/primitives/number-value-object.test.ts
 PASS  test/value-object/primitives/enum-value-object.test.ts
 PASS  test/value-object/primitives/date-value-object.test.ts
 ...
Test Suites: 10 passed, 10 total
Tests:       18 passed, 18 total
Snapshots:   0 total
Time:        6.438 s
```

---

### 2.4 Matriz de Cobertura de Código Obtenida

| Archivo / Componente | % Sentencias (Stmts) | % Ramas (Branch) | % Funciones (Funcs) | % Líneas (Lines) | Líneas No Cubiertas (`Uncovered Line #s`) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **All files (Global)** | **43.28%** | **60.00%** | **56.66%** | **43.28%** | N/A |
| `src/index.ts` | 62.50% | 100.00% | 25.00% | 62.50% | **8-10** |
| `src/quality/errores-object.ts` | **0.00%** | 100.00% | **0.00%** | **0.00%** | **2-6** |
| `src/quality/insecure-1-object.ts` | **0.00%** | 100.00% | 100.00% | **0.00%** | **1-10** |
| `src/quality/insecure-2-object.ts` | **0.00%** | 100.00% | **0.00%** | **0.00%** | **1-32** |
| `src/value-object/index.ts` | 100.00% | 100.00% | 100.00% | 100.00% | *Ninguna (100%)* |
| `src/value-object/primitives/date-value-object.ts` | **6.66%** | **0.00%** | **0.00%** | **6.66%** | **3-35** |
| `src/value-object/primitives/enum-value-object.ts` | 83.33% | **0.00%** | 100.00% | 83.33% | **11** |
| `src/value-object/primitives/number-value-object.ts` | 100.00% | 100.00% | 100.00% | 100.00% | *Ninguna (100%)* |
| `src/value-object/primitives/string-value-object.ts` | 100.00% | 100.00% | 100.00% | 100.00% | *Ninguna (100%)* |

<img width="654" height="455" alt="Captura de pantalla 2026-11-01 080349" src="https://github.com/user-attachments/assets/a7c71bbc-970b-43a5-a198-9aaacfe54821" />

### 2.5 Diagnóstico de Cobertura y Código Huérfano
1. **Módulo Crítico `src/quality/` (0% Cobertura):** Los archivos `errores-object.ts`, `insecure-1-object.ts` e `insecure-2-object.ts` carecen de archivos de prueba en `test/`. Contienen deliberadamente debilidades de código y patrones inseguros que constituyen el foco de análisis de la Parte 2.
2. **Módulo de Primitivas (`date-value-object.ts`):** Las líneas 3 a 35 no son alcanzadas por la suite de pruebas actual, dejando sin verificar métodos de parseo de fechas y manejo de formatos incorrectos.
3. **Manejo de Excepciones en Enums (`enum-value-object.ts`):** La línea 11 corresponde a la rama condicional que lanza la excepción ante valores no admitidos, la cual no cuenta con una prueba unitaria asociada.

---

## 3. PARTE 2: ANÁLISIS DE SEGURIDAD DE CÓDIGO ESTÁTICO (RAFAEL HENECHE)

### 3.1 Evaluación de Herramientas y Ciclo de Vida (Indicador IE3.2.1 - 50%)
Rafael Heneche evaluó técnica y operativamente el ecosistema de herramientas implementado para respaldar la migración de *Integration Solutions* a GitHub:

```text
+-------------------+---------------------------------------------+------------------------------------+
| HERRAMIENTA       | APLICACIÓN EN EL CICLO DE VIDA (SDLC)       | IMPACTO EN DETECCIÓN TEMPRANA      |
+-------------------+---------------------------------------------+------------------------------------+
| SonarCloud        | SAST en integración continua (CI/CD)        | Bloquea código con deuda técnica,   |
|                   | y validación de Pull Requests               | malas prácticas o bugs antes de ir |
|                   |                                             | a la rama principal (Quality Gate).|
+-------------------+---------------------------------------------+------------------------------------+
| Snyk              | SCA y SAST en desarrollo local (CLI)        | Identifica CVEs y componentes      |
|                   | y monitoreo en repositorio remoto           | desactualizados previo al build.   |
+-------------------+---------------------------------------------+------------------------------------+
| npm audit         | SCA nativo durante la fase de codificación  | Diagnóstico rápido de riesgos en   |
|                   | y gestión de dependencias directas          | el árbol de módulos npm.           |
+-------------------+---------------------------------------------+------------------------------------+
| GitHub Advanced   | Gobernanza continua, escaneo de secretos    | Frena la exposición de tokens      |
| Security          | y alertas automatizadas de Dependabot       | confidenciales en tiempo de push.  |
+-------------------+---------------------------------------------+------------------------------------+
```

* **Impacto en Detección Temprana (*Shift-Left*):** La adopción de estas herramientas traslada las verificaciones de seguridad desde la etapa previa al despliegue hacia el momento mismo de la escritura del código. Esto disminuye los costos de remediación en un orden de magnitud y elimina cuellos de botella en los lanzamientos.
* **Mejora Continua:** Mediante el concepto *Clean as You Code* de SonarCloud y las alertas automáticas con Pull Requests sugeridos de Snyk y Dependabot, el repositorio previene la degradación progresiva de la base de código.

---

### 3.2 Conexión y Configuración de SonarCloud
1. **Vinculación Web:** Se integró la cuenta de GitHub con la plataforma SonarCloud, importando el repositorio Fork `AUY1102-Pipeline-EV2`.
2. **Configuración en el Repositorio:** Se definió en la raíz el archivo `sonar-project.properties`:
   ```properties
   sonar.projectKey=<ORGANIZACION>_AUY1102-Pipeline-EV2
   sonar.organization=<ORGANIZACION>
   sonar.sources=src
   sonar.tests=test
   sonar.javascript.lcov.reportPaths=coverage/lcov.info
   sonar.sourceEncoding=UTF-8
   ```
3. **Métricas Obtenidas:** SonarCloud identificó los *Code Smells* situados en `src/quality/`, catalogando problemas de mantenibilidad y tipado dinámico inseguro (`any`).

---

### 3.3 Conexión y Ejecución de Snyk
1. **Vinculación Web:** Se añadió el repositorio en la consola SaaS de Snyk para escaneo continuo del archivo `package.json`.
2. **Autenticación y Escaneo por Consola (CLI):**
   * Autenticación exitosa mediante:
     ```bash
     npx snyk auth
     ```
   * Ejecución del análisis estático y de composición:
     ```bash
     npx snyk test
     ```
   * *Resultado:* Snyk auditó el grafo de dependencias, reportando vulnerabilidades conocidas con sus correspondientes identificadores CVE y recomendaciones de actualización de versión mínima no vulnerable.

---

### 3.4 Análisis y Remediación con `npm audit`
1. **Ejecución Inicial:**
   ```bash
   npm audit
   ```
   *Diagnóstico:* Se identificaron vulnerabilidades en dependencias transitivas asociadas a librerías de prueba y utilitarios de desarrollo.
2. **Remediación Automatizada:**
   ```bash
   npm audit fix
   ```
   *Efecto de la remediación:* npm actualizó las dependencias en `package-lock.json` a las versiones semánticas que resuelven los reportes de seguridad sin generar incompatibilidades de API.
3. **Remediación en Código Fuente (`src/quality/`):**
   * Se intervinieron las malas prácticas señaladas por el análisis estático en `src/quality/insecure-1-object.ts` y `errores-object.ts`, sustituyendo declaraciones inseguras de variables (`var`) por tipado inmutable (`const`/`let`) y saneando declaraciones para cumplir con el estándar TypeScript estricto.

---

### 3.5 Activación de GitHub Advanced Security
En el repositorio Fork (Ruta: *Settings* $\rightarrow$ *Code security and analysis*), Rafael Heneche habilitó:
* [x] **Dependency graph:** Mapeo visible del árbol completo de dependencias directas e indirectas.
* [x] **Dependabot alerts:** Sistema de notificación proactivo ante nuevas vulnerabilidades públicas en el catálogo de paquetes.
* [x] **Dependabot security updates:** Automatización que genera Pull Requests con la solución exacta apenas se descubre un parche de seguridad.
* [x] **Secret scanning:** Inspección continua en cada `git push` para bloquear e invalidar cadenas de conexión, API Keys o certificados comprometidos.

---

## 4. MATRIZ DE RASTREABILIDAD DE COMMITS Y COLABORACIÓN EN GITHUB

Para asegurar la transparencia en la autoría y la evaluación por parte del docente, los cambios fueron realizados en ramas separadas y fusionados mediante Pull Requests:

| Integrante | Rama de Trabajo | Commits Principales | Entregables Clave |
| :--- | :--- | :--- | :--- |
| **Isidro Benítez** | `feature/parte1-pruebas-unitarias` | `feat(tests): configurar script test:coverage...`<br>`docs: completar informe detallado de pruebas unitarias IE3.1.1...` | `package.json`, `.gitignore`, reporte de cobertura y métricas de 18 pruebas unitarias. |
| **Rafael Heneche** | `feature/parte2-seguridad-estatica` | `feat(security): configurar sonarcloud, snyk, dependabot...`<br>`fix(security): remediacion de vulnerabilidades con npm audit fix...` | `sonar-project.properties`, remediación en `package-lock.json`, refactor de `src/quality/` y evaluación IE3.2.1. |

---

## 5. CONCLUSIONES Y RECOMENDACIONES TÉCNICAS

1. **Cumplimiento de la Transformación Digital:** La migración manual a un flujo basado en GitHub con SonarCloud, Snyk y pruebas unitarias automáticas reduce drásticamente los ciclos de entrega de "Integration Solutions", asegurando despliegues frecuentes y con riesgo controlado.
2. **Deuda Técnica y Cobertura:** Si bien las 18 pruebas existentes aprobaron satisfactoriamente, la cobertura global del 43.28% evidencia que es indispensable escribir nuevas pruebas para los componentes en `src/quality/` y `date-value-object.ts`, asegurando que no ingresen regresiones a futuro.
3. **Gobierno de Seguridad:** La activación de GitHub Advanced Security y el monitoreo desatendido de Dependabot garantizan que el software se mantenga protegido a lo largo de todo su ciclo de vida, mitigando vulnerabilidades antes de que alcancen entornos productivos.
