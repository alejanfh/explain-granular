---
name: explain-granular
description: Explica un concepto técnico a nivel granular y concreto (código real, datos reales que viajan entre piezas, quién hace qué literalmente) cuando una explicación anterior se ha quedado abstracta. Úsala siempre que el usuario escriba /explain-granular, o diga cosas como "sigo sin entenderlo", "me parece muy abstracto", "más granular", "explícamelo a nivel código", "¿qué hace X en sí?", "¿X y Y son competidores?", "¿quién le dice a quién qué?", o reformule un concepto con dudas pidiendo confirmación. Especialmente útil para conceptos de backend, IA y agentes (LLM, tools, MCP, GenUI, LangGraph, AG-UI, RAG, gateways, auth) explicados a alguien que domina frontend. Úsala aunque no la nombre, si su mensaje muestra que la explicación previa no ha aterrizado.
---

# Explain granular

El objetivo es que el usuario pueda ver con sus propios ojos qué hace cada pieza, no que acepte una definición. Las explicaciones que fallan suelen tener tres problemas: usan palabras-paraguas ("capa", "patrón", "orquesta", "puente") sin enseñar la cosa literal; se apoyan en metáforas generales (restaurantes, menús, coches) que suenan bien pero no dicen qué ocurre en el ordenador; y no muestran los datos que viajan entre piezas. Esta skill corrige eso bajando siempre al nivel de "esto es lo que se escribe, esto es lo que se envía, esto es lo que se pinta".

El usuario de referencia es un Senior Frontend Engineer que está aprendiendo backend e ingeniería de IA. Domina React, Next.js, fetch, estado, componentes y eventos. Usa ese terreno como punto de apoyo, pero si el usuario tiene otro perfil, adapta el código de ejemplo a lo que él conoce.

Responde siempre en el idioma del usuario.

## Antes de explicar: localiza la confusión

Lee lo que el usuario ya ha dicho e identifica dos cosas:

1. Las piezas implicadas. Casi siempre hay entre dos y cuatro actores (por ejemplo: el LLM, la tool `present`, la librería assistant-ui). La confusión suele ser de responsabilidades: no sabe cuál decide, cuál transporta y cuál pinta.
2. Su modelo mental actual. Si lo ha formulado ("entonces GenUI le dice a assistant-ui que pinte una tabla"), señala exactamente qué parte es correcta y cuál no. No empieces de cero ignorando lo que ya tiene.

Si no se puede saber qué pieza le confunde, haz una sola pregunta corta. Si se puede deducir, no preguntes y explica.

## Estructura de la explicación

Sigue estos pasos en orden. Cada uno debe ser corto; la granularidad viene de lo concreto, no de la longitud.

### 1. Una línea de corrección o de foco

Si el usuario tenía un malentendido, abre corrigiéndolo en una frase directa: "Casi, pero no es GenUI quien decide: decide el LLM." Si no lo tenía, di en una frase qué vas a separar: "Te lo separo en las dos piezas."

### 2. Sin X / con X, en código

Para cada pieza, enseña primero lo que tendría que escribir el usuario si esa pieza no existiera, y después lo que escribe con ella. La diferencia entre los dos bloques ES la definición de la pieza. Ejemplo de forma:

```tsx
// Sin la librería: tú gestionas todo esto a mano
const [messages, setMessages] = useState([]);
const es = new EventSource("/api/chat"); // parsear cada chunk, auto-scroll, botón stop...

// Con la librería: una línea
<Thread />
```

Usa el lenguaje y las herramientas que el usuario ya conoce (para un frontend: React, fetch, useState, EventSource, JSON).

### 3. Sigue un ejemplo concreto de punta a punta, con los datos reales

Elige un caso pequeño y realista (con valores inventados pero concretos: "La Taberna, 25€") y enseña el payload literal en cada salto entre piezas:

- Qué se envía (por ejemplo, el cuerpo de la llamada a la API con `messages` y `tools`).
- Qué devuelve cada pieza (el JSON exacto del tool call).
- Qué hace la pieza final con eso (qué componente se renderiza).

Esto es lo más importante de la skill. Cuando el usuario ve el JSON que viaja, deja de necesitar metáforas.

### 4. Tabla "quién hace qué", con verbos literales

Una fila por pieza. La columna clave describe la acción con un verbo que se pueda observar: "emite un JSON con `$type: 'Table'`", "convierte ese JSON en `<Table />`", "recibe el texto token a token". Evita verbos vagos como "gestiona", "se encarga de", "orquesta" o "facilita" salvo que vayan seguidos de qué hace exactamente.

### 5. Resumen en una frase

Una sola frase que el usuario podría repetir a un compañero. Por ejemplo: "El LLM decide qué UI mostrar, GenUI es el formato en que lo expresa y assistant-ui es donde se pinta."

### 6. Invita a que lo reformule

Termina con una pregunta corta que le haga poner el concepto en sus propias palabras o predecir un caso: "¿Qué crees que pasa si el LLM decide no llamar a `present`?" Una sola pregunta.

## Cuando el usuario reformula

Este es el momento en que el aprendizaje se consolida. Responde así:

- Si es correcto, dilo claramente al principio ("Exacto") para que sepa que puede apoyarse en ese modelo.
- Añade como mucho uno o dos matices técnicos precisos, cada uno con su trozo de código o payload. Ejemplo: "Un matiz: no va en el prompt como texto, va en el parámetro `tools` de la llamada a la API", seguido del JSON de la llamada.
- Si una parte es incorrecta, señala solo esa parte y corrígela con un ejemplo concreto. No repitas toda la explicación.

## Analogías: solo después del código, y del terreno del usuario

Una analogía nunca sustituye al ejemplo concreto. Si la usas, ponla después de los pasos 2 y 3, y sácala del mundo técnico del usuario ("es como un mapa `{ tipo: Componente }` con validación Zod antes de renderizar") en lugar de del mundo cotidiano. Si la analogía no corresponde a algo que pasa realmente en el código, no la uses.

## Visuales

Si hay herramienta de visualización disponible y el concepto implica datos que viajan entre varias piezas, un diagrama de flujo con el payload en cada salto ayuda mucho. Si hay más de cuatro saltos, mejor un stepper animado que avance paso a paso mostrando quién envía, quién recibe y el JSON de ese paso. No hagas un visual cuando el código y la tabla ya lo dejan claro.

## Precisión

El código es el corazón de la explicación, así que tiene que ser fiel a cómo funciona la tecnología. Si tienes búsqueda web disponible y vas a mostrar nombres de APIs, imports o firmas concretas de una librería, compruébalos antes. Si muestras código simplificado o pseudocódigo para que se entienda mejor, dilo explícitamente ("simplificado") para que el usuario no lo copie como si fuera la API real. Es preferible un ejemplo un poco más genérico pero correcto que uno muy específico pero inventado.

## Ejemplo completo

En `references/example-genui.md` hay una conversación real donde este enfoque funcionó: el usuario no entendía la diferencia entre GenUI y assistant-ui, dos explicaciones abstractas no le sirvieron, y la versión granular sí. Léelo si quieres ver el tono y el nivel de detalle esperados.
