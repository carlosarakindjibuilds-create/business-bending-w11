# Sistema de Gestión Jukurrpa: Plenitude & Moola

## Registro de decisiones técnicas — Semana 11

Estas decisiones definen el alcance de la experiencia de simulación y sus controles mínimos de seguridad para el prototipo.

### DEC-01 — Interfaz unificada en una sola pantalla

- **Decisión:** concentrar la experiencia principal en una única pantalla interactiva de simulación.
- **Motivo:** permitir que la persona usuaria configure el esfuerzo voluntario, ejecute la simulación y revise sus resultados sin navegar entre vistas separadas.
- **Criterio de aceptación:** la pantalla permite iniciar y repetir una simulación, muestra claramente sus entradas y presenta el resultado y los estados de error sin perder el contexto.

### DEC-02 — Telemetría IoT simulada con variables dinámicas

- **Decisión:** implementar el componente de *Sensing* como un motor de simulación; no conectar sensores reales en esta etapa.
- **Motivo:** demostrar el flujo de telemetría de manera reproducible y evitar recopilar datos reales de personas durante el prototipo.
- **Comportamiento requerido:** generar valores variables de esfuerzo voluntario en cada ejecución —por ejemplo, duración, intensidad o tareas completadas— en lugar de reutilizar una salida fija. Mantener límites válidos y permitir que los resultados se expliquen a partir de los valores simulados.
- **Criterio de aceptación:** ejecuciones sucesivas pueden producir telemetría distinta dentro de los rangos definidos, y cada salida indica que su origen es simulado.

### DEC-03 — Validación y sanitización de entradas

- **Decisión:** validar y sanitizar estrictamente todas las entradas controlables por el usuario en la interfaz antes de usarlas o mostrarlas.
- **Controles requeridos:** validar tipo, formato, longitud y rango con reglas permitidas explícitas; rechazar entradas inválidas con mensajes comprensibles; codificar los datos al representarlos en la interfaz y no interpretarlos como contenido ejecutable.
- **Límite de seguridad:** la validación del frontend no sustituye la validación en cualquier servicio o backend que se incorpore posteriormente. No confiar en entradas del navegador ni construir consultas o comandos concatenando datos proporcionados por usuarios.
- **Criterio de aceptación:** pruebas con valores fuera de rango, formatos inesperados y texto no permitido no ejecutan contenido, no alteran la simulación y producen errores visibles y controlados.

## Checklist de verificación — Security Floor

- [x] **Archivos locales de secretos excluidos:** `.gitignore` excluye `.env` y `.env.*`, y permite únicamente `.env.example` como plantilla. Las llaves API se guardarán solo en archivos `.env` locales ignorados; nunca en el código fuente ni en la plantilla. Esta regla de exclusión no elimina secretos que ya estuvieran versionados.
- [x] **Sin datos reales de personas en el diseño del prototipo:** la telemetría y los ejemplos son simulados; no se deben incorporar registros, identificadores directos ni datos sensibles reales de voluntarios o ciudadanos atendidos.
- [ ] **Sanitización implementada y probada:** confirmar mediante pruebas de entradas inválidas antes de considerar terminado el control.
- [ ] **Revisión antes de compartir:** comprobar que no se hayan añadido llaves, archivos `.env` reales ni datos personales al control de versiones.

> Las casillas marcadas confirman las decisiones y reglas observables en el alcance documentado y en `.gitignore`; no sustituyen una auditoría del historial de Git ni pruebas de seguridad de una implementación.

### Validación del Piso de Seguridad

La caja de comentarios está implementada para bloquear caracteres `<` y `>`, incluidos los de etiquetas como `<script>`, mostrar una alerta y rechazar la entrada. La verificación manual en navegador queda pendiente; no se marca como confirmada hasta probar la interacción en la aplicación.