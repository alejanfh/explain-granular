# /explain-granular

Skill para Claude que explica conceptos técnicos a nivel de código: qué se escribe, qué JSON viaja entre las piezas y quién hace qué.

La hice porque, aprendiendo backend e IA viniendo de frontend, pedía que me explicaran algo (GenUI vs assistant-ui, MCP, RAG...) y me llegaba una analogía de restaurantes con palabras como "capa" u "orquesta". Lo leía y seguía sin saber qué pasaba en el ordenador.

## Qué hace

Cuando una explicación se queda abstracta, la rehace en este orden:

1. Corrige en una frase lo que habías entendido mal.
2. Enseña el código sin la pieza y con ella. Lo que cambia entre los dos bloques es lo que hace la pieza.
3. Sigue un ejemplo pequeño de punta a punta con el payload real de cada salto.
4. Hace una tabla de quién hace qué con verbos que se puedan ver ("emite este JSON", "renderiza `<Table />`").
5. Lo resume en una frase y te pide que lo expliques tú.

Las analogías solo entran después del código.

Se activa con `/explain-granular` o sola, cuando escribes cosas como "sigo sin entenderlo" o "explícamelo a nivel código".

## Instalación

Claude Code:

```bash
git clone https://github.com/alejanfh/explain-granular.git ~/.claude/skills/explain-granular
```

claude.ai o Claude Desktop: descarga el repo como ZIP y súbelo desde la sección de Skills en los ajustes.

## Ejemplo

[`references/example-genui.md`](references/example-genui.md) tiene la conversación que dio origen a la skill: dos explicaciones abstractas sobre GenUI y assistant-ui que no funcionaron y la versión granular que sí.

La skill está escrita en español, pero responde en el idioma de quien la usa.
