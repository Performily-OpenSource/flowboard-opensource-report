# Flowboard

Plataforma web de gestión de recursos humanos para organizaciones peruanas en crecimiento. Consolida en una fuente única de verdad la ficha del colaborador, la estructura organizacional, la asistencia, los beneficios, las boletas de pago y las solicitudes, y expone esa información directamente al trabajador mediante un módulo de autogestión.

Producto de la startup **Performily**, desarrollado como trabajo final del curso 1ASI0729 Desarrollo de Aplicaciones Open Source, NRC 7729, ciclo 2026-20, Universidad Peruana de Ciencias Aplicadas.

---

## El problema

Las organizaciones de 50 a 500 colaboradores gestionan la información de su personal en hojas de cálculo aisladas, documentos físicos y sistemas que no se comunican entre sí. Eso obliga a Recursos Humanos a reingresar los mismos datos en varios archivos, produce información inconsistente y deja al colaborador sin ninguna vía para consultar por sí mismo su remuneración, sus beneficios o su saldo de vacaciones.

Las plataformas del mercado se construyeron alrededor del motor de remuneraciones y de las necesidades del área administrativa, y dejan al colaborador como receptor pasivo de información que no puede consultar por su cuenta.

## La propuesta

Fuente única de verdad sobre el vínculo laboral, autogestión real del colaborador, y aprobaciones ruteadas según la jerarquía organizacional declarada en el sistema.

## Segmentos objetivo

| Segmento | Quién es | Qué hace en la plataforma |
| ----- | ----- | ----- |
| Personal de Recursos Humanos | Analista, coordinador o jefe de RR.HH. de una organización de 50 a 500 colaboradores | Administra la ficha del personal, la estructura organizacional, la asistencia, los beneficios y la publicación de boletas |
| Colaborador | Personal operativo, administrativo, comercial o técnico | Consulta su propia información laboral y presenta solicitudes |
| Colaborador con personal a cargo | Subperfil del segmento anterior, no un segmento aparte | Además resuelve las solicitudes de quienes le reportan. Su condición de aprobador se deriva de tener subordinados asignados, no se configura como permiso |

---

## Alcance

### Lo que Flowboard hace

- **Gestión del colaborador y estructura organizacional.** Datos personales, puesto, área, fecha de ingreso, tipo de contrato y estado (activo, cesado, suspendido). Áreas, departamentos y jerarquía entre jefe y colaborador. Esa jerarquía es la que permite rutear las aprobaciones.
- **Asistencia.** Registro de faltas, puntualidad y horas trabajadas, con las horas efectivas y el sobretiempo derivados de la jornada esperada del puesto.
- **Pagos y beneficios.** Remuneración asignada y sueldo mínimo de referencia por puesto. Gratificaciones, canastas, vales de consumo y licencias. Saldo de vacaciones con días acumulados y días usados.
- **Solicitudes.** Flujo Pendiente, Aprobado y Rechazado para vacaciones, licencias y permisos, con ruteo automático al aprobador y notificación por correo.
- **Bienestar laboral.** Registro y clasificación de lecturas ambientales de los espacios de trabajo según umbrales configurables por la organización.

### Tres decisiones de alcance

Son explícitas y no se contradicen en ninguna parte del proyecto.

1. **El módulo de Pagos es de consulta, no de cálculo.** Flowboard muestra la remuneración asignada y el sueldo mínimo por puesto. No calcula remuneraciones, no aplica descuentos ni aportes, no emite boletas ni archivos para entidades recaudadoras, y no reemplaza al sistema contable que la organización ya utiliza.
2. **La asistencia es solo registro.** No deriva descuentos, bonificaciones ni pagos automáticos.
3. **No hay migración ni importación masiva de datos históricos.** La carga inicial es manual. La migración asistida es parte del roadmap posterior a este ciclo.

### Fuera de alcance

Motor de cálculo de planilla y cumplimiento tributario. Hardware biométrico o relojes de asistencia físicos. Aplicaciones móviles nativas, porque la experiencia móvil se resuelve con diseño responsive. Firma electrónica legalmente vinculante. Reclutamiento y selección, evaluación de desempeño por competencias y encuestas de clima. Integraciones con ERP contables de terceros.

---

## Modelo de dominio

El dominio se divide en siete bounded contexts, cada uno con su propio modelo, su propio lenguaje y su propia frontera de consistencia.

| Bounded context | Agregado raíz | Tipo de subdominio |
| ----- | ----- | ----- |
| IAM | `UserAccount` | Genérico |
| Workspace | `Employee` | Principal, upstream de todos los demás |
| Attendance | `AttendanceRecord` | Soporte |
| Request | `Request` | Principal |
| Benefits | `BenefitAssignment`, `VacationBalance` | Soporte |
| Payroll | `Payslip` | Soporte |
| Wellbeing | `Office` | Soporte |

**Regla de integración.** Un contexto nunca guarda un objeto de otro contexto, solo su identificador (`EmployeeId`, `AreaId`, `PositionId`, `RequestId`), definido en el shared kernel. En la base de datos esas columnas no llevan llave foránea. Las llaves foráneas existen únicamente dentro de un mismo contexto.

---

## Productos digitales

| Producto | Stack | Estado |
| ----- | ----- | ----- |
| Landing Page | HTML5, CSS3 y JavaScript, estático y responsive | Desplegado en el Sprint 1 |
| Web Application | Angular con TypeScript y Angular Material | Diseño completo, implementación pendiente |
| RESTful API | Java con Spring Boot y Spring Data JPA, documentado con OpenAPI vía Swagger | Diseño completo, implementación pendiente |
| Base de datos | MySQL 8 | Diseño completo, implementación pendiente |

### Integraciones externas

- Proveedor transaccional de correo para las notificaciones a solicitantes y aprobadores.
- API pública de feriados nacionales para el cómputo de días hábiles en las solicitudes de vacaciones, en evaluación.

---

## Repositorios

| Repositorio | Contenido |
| ----- | ----- |
| [`flowboard-landing-page`](https://github.com/Performily-OpenSource/flowboard-landing-page) | Landing Page estático, desplegado en GitHub Pages |
| `flowboard-report` | Informe académico del proyecto |
| `flowboard-webapp` | Web Application en Angular |
| `flowboard-api` | RESTful API en Spring Boot |

Landing Page desplegada: https://performily-opensource.github.io/flowboard-landing-page/

---

## Artefactos del proyecto

| Artefacto | Herramienta | Enlace |
| ----- | ----- | ----- |
| Wireframes del Landing Page y de la Web Application | Figma | https://www.figma.com/design/KJsdWA2t4Ua97beOmCOeFE/ |
| Mock-ups y prototipo navegable | Figma | https://www.figma.com/design/enPdopE6jbleKgX3BrgiiP/ |
| Wireflow diagrams y user flow diagrams | FigJam | https://www.figma.com/board/Y6swoxttKzSJbRY8nsQwI8/ |
| Big Picture Event Storming | Miro | https://miro.com/app/board/uXjVHoF1UYQ=/?share_link_id=314492633919 |
| Design-Level Event Storming | Miro | https://miro.com/app/board/uXjVHpKyn4g=/?share_link_id=844689573695 |
| Diagramas C4, de clases y de base de datos | Structurizr y PlantUML | [Carpeta en Drive](https://drive.google.com/drive/folders/1camvi5N7z_Omp7n_XMW02Gk4gKGD_qtN) |
| Product Backlog y Sprint Backlog | Trello | https://trello.com/b/KZiuVfYX/flowboard-product-backlog |

El diseño de la Web Application son 65 pantallas codificadas de `WA-01` a `WA-65`. Ese mismo código se usa en los wireframes, los mock-ups, el prototipo, los wireflows y los user flows, de modo que cualquier pantalla se rastrea a través de los cinco artefactos con un solo identificador.

---

## Convenciones de desarrollo

### Control de versiones

GitFlow, Conventional Commits y Semantic Versioning.

- `main` contiene solo código estable y listo para producción.
- `develop` es el eje de integración de las nuevas características.
- `feature/<descripcion-en-kebab-case>` nace de `develop` y vuelve mediante Pull Request.
- `release/<version>` prepara una entrega e integra en `main` y en `develop`.
- `hotfix/<fix-error>` nace de `main` para correcciones urgentes e integra en ambas ramas.

Formato de commit: `<tipo>(<alcance>): <descripción>`, con tipos `feat`, `fix`, `docs`, `style`, `refactor`, `test` y `chore`. La descripción va en presente imperativo y en inglés.

```
feat(hero): add background image
fix(index): fix menu error
```

### Código

Todo el código se escribe en inglés: nombres de variables, funciones, clases, archivos y comentarios. Se evitan abreviaturas ambiguas y se priorizan nombres que revelen la intención.

- HTML5 y CSS3 siguen la Google HTML/CSS Style Guide: minúsculas en etiquetas y atributos, comillas dobles en los valores y sangría de dos espacios.
- JavaScript y TypeScript siguen la Google JavaScript Style Guide: `camelCase` para variables y funciones, `PascalCase` para clases, y `const` y `let` en lugar de `var`.

### Internacionalización y accesibilidad

El idioma por defecto de la interfaz y de la documentación de todos los productos es **inglés**. Se soportan English (`en_US`) y Latin American Spanish (`es_419`) bajo i18n. El Landing Page y la Web Application incorporan atributos ARIA.

La paleta parte de la semilla Material `#39608F` con tipografía Inter, y los contrastes están verificados contra los criterios 1.4.3 y 1.4.11 de la WCAG 2.1.

---

## Protección de datos

La plataforma expone información remunerativa, por lo que el tratamiento de datos personales se sujeta a la **Ley N.° 29733, Ley de Protección de Datos Personales**, y su reglamento.

El control de acceso por rol es un requisito legal, no un detalle técnico: cada colaborador ve únicamente su propia información. La restricción se aplica en el servidor, no solamente en la interfaz, y todas las vistas de la Web Application se marcan con `noindex, nofollow`. Solo el Landing Page es indexable.

---

## Equipo

| Integrante | Código |
| ----- | ----- |
| Ávila De La Cruz, Darío Fabián | u202412270 |
| Diaz Villalba, Diego Alonso | u202412663 |
| Galvez Meza, Salym Pool | u202419655 |
| Li Gayoso, Diana Carolina | u202415749 |
| Vasquez Llave, Oscar Lizandro | u202410478 |

Docente: Hugo Allan Mori Paiva.

---

## Licencia

Proyecto académico desarrollado con tecnologías open source.
