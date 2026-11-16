# /explain-granular

Una skill para Claude (Claude Code, claude.ai, Claude Desktop) que explica conceptos técnicos bajando al nivel del código: qué se escribe, qué JSON viaja entre piezas y quién hace qué, literalmente.

La hice porque me pasaba mucho al aprender backend e IA viniendo de frontend: pedía que me explicaran algo (GenUI vs assistant-ui, MCP, RAG, tools...) y la respuesta era una analogía de restaurantes y palabras como "capa", "orquesta" o "puente". Sonaba bien, pero seguía sin saber qué pasaba en el ordenador.

## Qué hace

Cuando una explicación se queda abstracta, la skill la rehace siempre con la misma estructura:

1. **Corrige el malentendido** en una frase, si lo había.
2. **Sin X / con X, en código.** Lo que tendrías que escribir sin la pieza y lo que escribes con ella. La diferencia es la definición.
3. **Un ejemplo de punta a punta con los datos reales**: el payload que se envía, el JSON que vuelve y el componente que se pinta.
4. **Tabla "quién hace qué"** con verbos que se pueden observar ("emite un JSON", "renderiza `<Table />`"), no "gestiona" ni "se encarga de".
5. **Resumen en una frase** que puedas repetir a un compañero.
6. **Una pregunta** para que lo reformules con tus palabras.

Las analogías solo entran después del código, y si no corresponden a algo que pasa de verdad, no se usan.

## Cuándo se activa

- Escribiendo `/explain-granular`.
- O sola, cuando dices cosas como "sigo sin entenderlo", "muy abstracto", "explícamelo a nivel código", "¿X y Y son competidores?".

## Instalación

**Claude Code**

```bash
git clone https://github.com/alejanfh/explain-granular.git ~/.claude/skills/explain-granular
```

**claude.ai / Claude Desktop**

Descarga el repo como ZIP y súbelo desde la sección de Skills en los ajustes de Claude.

## Ejemplo

En [`references/example-genui.md`](references/example-genui.md) está la conversación que originó la skill: dos explicaciones abstractas que no funcionaron sobre GenUI y assistant-ui, y la versión granular que sí.

## Estructura

```
explain-granular/
├── SKILL.md
└── references/
    └── example-genui.md
```

La skill está escrita en español, pero responde en el idioma del usuario.
