## LAB # 20 · Global Chess Passport

### A.1 · Encabezado

| Campo | Respuesta |
|---|---|
| Autor | Trabajo individual |
| Caso | Algorand Foundation + World Chess — Global Chess Passport |
| Estado real | Propuesta y desarrollo a pequeña escala, según la [página oficial](https://algorand.co/universal-chess-passport); las capacidades entre plataformas son promesas |

### Estado real: hecho frente a promesa

Encontré una [propuesta publicada en abril de 2025](https://algorand.co/blog/world-chess-and-the-algorand-foundation-propose-leveling-the-playing-field-with-a-chess-passport), no un pasaporte desplegado para todos los ajedrecistas. La [página oficial](https://algorand.co/universal-chess-passport) dice que el sistema sigue en desarrollo a pequeña escala. Por eso traté la portabilidad, la divulgación selectiva y la revocación entre plataformas como capacidades propuestas; no afirmé que ya funcionen en producción ni que FIDE las haya adoptado.

Mi criterio para separar ambas categorías fue exigir una demostración o una afirmación explícita de funcionamiento actual. El anuncio describe beneficios condicionados a la adopción; eso permite analizar riesgos de diseño, pero no medir usuarios, transacciones ni resultados reales. La diferencia importa porque un sistema todavía conceptual puede omitir decisiones difíciles que serían decisivas al desplegarlo.

### A.2 · Triángulo

| Credencial hipotética | Emisor | Holder | Verificador |
|---|---|---|---|
| Rating ≥ 2000 | Plataforma o federación que certificó ese rating | Jugador, en su billetera | Otro torneo o plataforma que exige el umbral |
| Buena conducta | Plataforma que investigó y firma la afirmación | Jugador, en su billetera | Otra plataforma o torneo que decide admitirlo |

Yo separé las funciones: el emisor firma, el jugador conserva la credencial y el verificador comprueba firma y estado. No supuse que FIDE sea emisora del proyecto: el [whitepaper](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf) la menciona como posibilidad, no como participante comprometido.

Para «rating ≥ 2000», quien calcula el rating debe responder por su exactitud; una plataforma receptora no puede reemplazar esa confianza mirando solo una firma válida. Para «buena conducta», la definición de la falta y la posibilidad de impugnarla importan tanto como la criptografía, porque una sanción podría viajar entre servicios.

### A.3 · Quién ve qué

| Actor | Puede ver | No puede concluir solo con eso | Base |
|---|---|---|---|
| Emisor | Lo que verificó para emitir y el estado que administra | Cada uso posterior, si no se le consulta directamente | [Whitepaper](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf); límite inferido |
| Jugador (holder) | Sus credenciales y presentaciones | Que cualquier verificador aceptará al emisor | [Whitepaper](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf); límite inferido |
| Verificador | Atributos presentados, emisor, firma y estado | Atributos ocultos, si se implementa la divulgación propuesta | [Whitepaper](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf); condicionado |
| Cadena pública | Datos que se publiquen en el registro propuesto | Datos personales fuera de cadena; campos exactos aún no especificados | [Whitepaper](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf) |
| Quien roba la frase de 24 palabras | Podría controlar llaves recuperables de la billetera | Que necesariamente pueda leer todo dato privado sin acceso adicional | [Whitepaper](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf); inferencia limitada |

### A.4 · Divulgación, revocación y correlación

| Pregunta | Respuesta | Evidencia y concepto |
|---|---|---|
| ¿Revela solo un atributo? | **No se sabe si está implementado.** | El [whitepaper](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf) dice «should be used where possible», sin formato concreto; §4.7. |
| ¿Quién revoca y cómo? | **El emisor, según la propuesta; mecanismo no especificado.** | El [whitepaper](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf) dice «VCs can be revoked by the issuer»; no define lista o contrato; §4.9. |
| ¿Se pueden unir usos? | **Sí, si se reutiliza una DID estable.** | Inferí el riesgo de la identidad «one secure digital identity» propuesta en el [blog del arquitecto](https://algorand.co/blog/why-im-excited-about-a-decentralized-identity-system-for-chess); §4.10. |

La divulgación selectiva reduciría lo que recibe cada verificador, pero no demostraría por sí sola anonimato. Si dos servicios ven el mismo identificador o un dato estable de la credencial, podrían vincular sus registros. Tampoco hay un mecanismo de revocación suficientemente detallado en las fuentes para afirmar que el emisor no sabría cuándo se consulta el estado.

### A.5 · Qué se rompe y un cambio de diseño

Primero, una revocación errónea de «buena conducta» podría causar rechazos en otras plataformas: la portabilidad también propagaría el error. Segundo, una DID estable ante todos los verificadores permitiría correlacionar el historial del jugador aunque oculte algunos atributos. Yo propondría credenciales con identificadores distintos por relación y una vía de apelación documentada para revocaciones. Es una mejora sugerida, no una función observada. Atacaría la correlación de §4.10 y el daño de una revocación incorrecta de §4.9, aunque tendría que comprobarse que diferentes verificadores sigan aceptando esas credenciales.

**Fuentes de §9:** [caso](https://algorand.co/universal-chess-passport), [anuncio](https://algorand.co/blog/world-chess-and-the-algorand-foundation-propose-leveling-the-playing-field-with-a-chess-passport), [whitepaper](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf), [blog del arquitecto](https://algorand.co/blog/why-im-excited-about-a-decentralized-identity-system-for-chess).
