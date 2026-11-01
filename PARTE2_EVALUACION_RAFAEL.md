# Evaluación de Métodos y Herramientas de Calidad y Seguridad (Indicador IE3.2.1)
**Responsable:** Rafael Heneche  
**Asignatura:** Ciclo de Vida del Software I (AUY1102)  
**Evaluación:** Evaluación Parcial N° 2  

---

## 1. Evaluación Comparativa de Métodos y Herramientas en el Ciclo de Vida del Software

Para dar respuesta a los requerimientos de la transformación digital de la empresa *"Integration Solutions"*, se evalúa técnicamente el ecosistema de herramientas implementado:

| Herramienta | Tipo de Análisis | Fase del Ciclo de Vida (SDLC) | Impacto en Detección Temprana (*Shift-Left*) | Mecanismo de Mejora Continua |
| :--- | :--- | :--- | :--- | :--- |
| **SonarCloud** | SAST (Static Application Security Testing) y Calidad de Código | Codificación, Pull Request y CI/CD | Bloquea la introducción de deuda técnica, malos olores (*code smells*) y vulnerabilidades antes del despliegue mediante *Quality Gates*. | Establece métricas comparativas entre código nuevo y código heredado (*Clean as You Code*), forzando refactorizaciones progresivas. |
| **Snyk** | SCA (Software Composition Analysis) y SAST | Desarrollo local (CLI), Repositorio y CI/CD | Identifica vulnerabilidades conocidas (CVE) en dependencias de terceros de forma previa a la integración a ramas protegidas. | Monitoreo continuo de dependencias en producción y generación automática de Pull Requests con versiones parchadas. |
| **npm audit** | SCA Nativo del gestor de paquetes | Desarrollo local y empaquetado | Permite al desarrollador auditar el árbol de módulos instalados inmediatamente después de un `npm install`. | Detección rápida y resolución automatizada de incidencias menores mediante `npm audit fix`. |
| **GitHub Advanced Security** | Detección de Secretos y Gobierno de Dependencias | Repositorio central (Git Push y Pull Requests) | Evita la fuga de credenciales críticas en tiempo real mediante *Secret Scanning* y alerta sobre librerías comprometidas vía Dependabot. | Automatiza la remediación con PRs de Dependabot y mantiene un inventario vivo de dependencias a través del *Dependency Graph*. |

---

## 2. Impacto en la Detección Temprana de Errores (*Shift-Left Testing*)

La integración coordinada de estas cuatro herramientas permite transformar el flujo de trabajo manual y lento de "Integration Solutions" en un flujo automatizado de alta confiabilidad:

1. **Reducción Exponencial de Costos:** Corregir un fallo de seguridad o un defecto de lógica en la estación de trabajo del programador (mediante `npx snyk test` y `npm audit`) es hasta 30 veces más económico que solucionarlo una vez desplegado en producción.
2. **Prevención de Brechas Críticas:** El escaneo de secretos de GitHub y el análisis estático de SonarCloud impiden que credenciales corporativas o patrones inseguros (como inyecciones SQL o deserializaciones no seguras) salgan del entorno de pruebas.
3. **Validación Automática en Pull Requests:** Las herramientas operan como filtros de calidad (*gates*), garantizando que ningún código sea fusionado a `main` si no cumple con los umbrales mínimos de seguridad y mantenibilidad.

---

## 3. Evidencias de Configuración y Ejecución Práctica

### 3.1 Conexión con SonarCloud
* **Proyecto Vinculado:** `AUY1102-Pipeline-EV2`
* **Archivo de Configuración:** `sonar-project.properties` creado en la raíz del repositorio.
* **Métricas Analizadas:**
  * Duplicación de código: 0.0%
  * Mantenibilidad (*Code Smells*): Identificados en el módulo `src/quality/`.
  * Cobertura asociada: Se vinculó la ruta `coverage/lcov.info` generada en la Parte 1.

### 3.2 Conexión con Snyk
* **Proyecto Vinculado:** Importado exitosamente en la consola web de Snyk.
* **Ejecución CLI:** `npx snyk test` ejecutado localmente sobre el árbol de dependencias de Node.js.
* **Resultado del Escaneo:** Se identificaron vulnerabilidades clasificadas por criticidad con sus respectivos identificadores de la base de datos de Snyk.

### 3.3 Auditoría de Seguridad con `npm audit`
* **Comando:** `npm audit`
* **Resultado Inicial:** Se detectaron incidencias en dependencias transitivas del archivo `package-lock.json`.

### 3.4 Activación de GitHub Advanced Security
En la pestaña *Settings* -> *Code security and analysis* del Fork se habilitaron:
* [x] **Dependency graph**: Activo para inspección de paquetes.
* [x] **Dependabot alerts**: Activo para notificación de CVEs.
* [x] **Dependabot security updates**: Activo para remediación desatendida.
* [x] **Secret scanning**: Activo para prevenir filtración de tokens y credenciales.

---

## 4. Remediación Implementada

Para evidenciar la capacidad de resolución práctica requerida por la pauta:
1. **Remediación en Dependencias:**
   * Se ejecutó el comando `npm audit fix`, actualizando paquetes vulnerables a versiones con parches de seguridad oficiales sin introducir incompatibilidades de versiones mayores.
2. **Remediación en Código Fuente:**
   * Se refactorizaron malas prácticas de codificación detectadas en `src/quality/`, sustituyendo declaraciones obsoletas por variables inmutables (`const`/`let`), asegurando el tipado estricto de TypeScript y mitigando posibles vectores de falla detectados por el análisis estático.