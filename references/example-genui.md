# Ejemplo: GenUI vs assistant-ui

Este ejemplo muestra por qué hace falta la explicación granular y qué forma tiene.

## Lo que no funcionó

Pregunta: "¿assistant-ui es otro competidor de GenUI? ¿O seleccionas uno u otro?"

Primera respuesta (demasiado abstracta): una tabla con "GenUI es un patrón / assistant-ui es una librería", una analogía de restaurante ("GenUI es el menú, assistant-ui el restaurante entero") y un diagrama de cajas anidadas. El usuario respondió reformulando con dudas: "¿GenUI es el que le dice a assistant-ui que pinte una tabla?"

Segunda respuesta (todavía abstracta): corrigió bien el malentendido ("no decide GenUI, decide el LLM") pero lo explicó con palabras como "puente" e "idioma". El usuario: "sigue pareciéndome muy abstracto, sigo sin entender qué hace GenUI y assistant-ui en sí".

## Lo que funcionó

1. Separar las piezas: "Vale, vamos a nivel código. Te lo separo en dos partes."

2. Sin/con para assistant-ui (simplificado):

```tsx
// Sin assistant-ui escribes tú todo esto:
const [messages, setMessages] = useState([]);
const [isStreaming, setIsStreaming] = useState(false);
const es = new EventSource("/api/chat");
// parsear cada chunk, auto-scroll, botón stop, regenerar,
// markdown, mostrar tool calls, accesibilidad...

// Con assistant-ui:
<AssistantRuntimeProvider runtime={runtime}>
  <Thread />
</AssistantRuntimeProvider>
```

3. Qué es GenUI literalmente: una tool más que se le pasa al LLM. Se registra un catálogo de componentes (Table, Card, Chart...) y el LLM puede llamar a `present`.

4. El payload real que emite el LLM:

```json
{
  "$type": "Table",
  "columns": ["Nombre", "Precio", "Valoración"],
  "rows": [["La Taberna", "25€", "4.5"], ["El Rincón", "35€", "4.8"]]
}
```

Y el mapeo que hace el front: `$type: "Table"` → `<Table {...props} />`.

5. Tabla de quién hace qué:

| Pieza | Qué hace literalmente |
| --- | --- |
| assistant-ui | Da `<Thread />`, `<Composer />`, `<Message />`: scroll, streaming, input, stop, markdown |
| GenUI (`present`) | Una tool más del LLM; su llamada trae un JSON que se convierte en componentes React |
| El LLM | Decide si llama a `present` con un Table, con un Chart, o si responde solo con texto |

6. Resumen: "assistant-ui es el contenedor del chat. GenUI es una tool para que el LLM devuelva `{ $type: 'Table', rows: [...] }` en lugar de una tabla en texto."

## La reformulación del usuario y la confirmación

Usuario: "O sea, sin GenUI assistant-ui recibiría texto, por ejemplo en puntos. Con GenUI se le envía al LLM eso de 'tienes una herramienta present y puedes crear componentes con JSON', y el LLM dice 'lo mandaría en texto, pero como tengo GenUI voy a mandar esto'."

Respuesta: "Exacto. Un matiz: no va en el prompt como texto, va en el parámetro `tools` de la llamada a la API", seguido del JSON de la llamada con `messages` y `tools`, y el contraste final: sin GenUI devuelve markdown con bullets; con GenUI devuelve un tool call a `present` que se pinta como tabla real.

Fíjate en que la confirmación empieza con "Exacto", añade un solo matiz y lo demuestra con un payload. No repite la explicación entera.
