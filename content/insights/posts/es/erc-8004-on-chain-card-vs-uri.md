# Identidad on-chain vs. información off-chain

Cuando me cruzo con un agente, ¿dónde encuentro su identificación?

Luego de ocho meses del despliegue en mainnet nos quisimos hacer una pregunta: ¿dónde vive realmente una agent card legible? ¿En el registry, en el archivo URI, o en ningún lado?

Para responderla nos basamos en esta información:

- **Fecha del snapshot:** 24 de agosto de 2026.
- **Universo:** solo Identity en mainnet, en ocho chains EVM: Ethereum L1, Base, Polygon, BNB Chain, Arbitrum, Celo, Gnosis, X Layer.
- **Stock:** **441.794** agentes.
- **Definición de “card usable”:** un nombre no vacío **y** una description de al menos 10 caracteres. Un nombre solo es una etiqueta, no una card.

## Información on-chain

| Campo on-chain | Agentes | % de 441.794 |
| --- | ---: | ---: |
| Tiene perfil chain | 441.794 | 100% |
| Nombre no vacío | 441.794 | 100% |
| Description ≥ 10 caracteres (**card usable**) | **205.564** | **46,5%** |
| Nombre sin description usable | 236.230 | 53,5% |
| Description vacía | 234.253 | 53,0% |
| Description de 1–9 caracteres | 1.977 | 0,4% |
| Image no vacía | 172.868 | 39,1% |
| Nombre + description + image | 171.269 | 38,8% |
| Services, skills o tags en el perfil chain | **0** | 0% |

## Información off-chain (registro URI)

| URI off-chain | Agentes | % |
| --- | ---: | ---: |
| Declara un `agentURI` | 391.235 | 88,6% |
| Perfil URI parseado | **371.473** | **84,1%** |
| Perfil URI usable (nombre + description ≥ 10) | **352.017** | **79,7%** |
| Perfil URI con image | 306.056 | 69,3% |
| Perfil URI con **services** | **181.132** | **41,0%** |
| Documento almacenado detrás del puntero | 387.211 | 87,6% |

## Información off-chain (feedback)

| Perfil de origen feedback | Agentes |
| --- | ---: |
| Cualquier perfil de origen feedback | **9.463 (2,1%)** |
| Nombre + description usables | 5.456 (1,2%) |

## Tres orígenes, un hueco

| Bucket | Agentes | % |
| --- | ---: | ---: |
| Card on-chain usable | 205.564 | 46,5% |
| Perfil URI (cualquier fill) | 371.473 | 84,1% |
| Perfil feedback (cualquier fill) | 9.463 | 2,1% |
| Chain usable **y** perfil URI | 204.323 | 46,2% |
| Los tres orígenes | 3.983 | 0,9% |
| Solo chain usable (sin URI, sin perfil feedback) | 1.240 | 0,3% |
| Sin chain usable, solo perfil URI | 163.910 | 37,1% |
| Sin chain usable, pero perfil URI o feedback | **169.389** | **38,3%** |
| Sin chain usable, sin perfil URI, sin perfil feedback | **66.841** | **15,1%** |

## Mismo estándar, cards distintas

| Chain | Agentes | % del stock | On-chain usable | Perfil URI | URI usable | Perfil feedback |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| BNB Chain | 296.616 | 67,1 | **52,6%** | **94,7%** | 93,8% | 0,2% |
| Base | 67.701 | 15,3 | 42,1% | 64,9% | 57,6% | **8,9%** |
| Ethereum L1 | 50.397 | 11,4 | **21,2%** | 40,4% | 29,8% | 0,9% |
| X Layer | 11.170 | 2,5 | **0,3%** | 97,0% | 42,3% | 0,0% |
| Celo | 9.779 | 2,2 | **90,1%** | 98,0% | 96,7% | **14,5%** |
| Gnosis | 4.115 | 0,9 | 6,4% | 99,5% | 96,9% | **24,1%** |
| Arbitrum | 1.387 | 0,3 | 86,7% | 97,4% | 91,1% | 2,1% |
| Polygon | 629 | 0,1 | 13,5% | 82,7% | 67,7% | 14,1% |

## Análisis

Nuestro análisis anterior preguntaba dónde viven las identidades y la reputation de los agentes ERC-8004. Este hace una pregunta más estrecha: cuando te cruzás con un agente ERC-8004, **¿dónde está realmente su identidad?**

**El marco del estándar.** Identity es un ERC-721 más un puntero. Desde su propia concepción el estándar fue diseñado para almacenar la identidad de forma dual: una parte en el contrato on-chain y otra en el puntero externo, donde el agente tiene más libertad para describir skills, services y endpoints. Incluso puede usar feedback para adjuntar un tercer puntero, que a veces alberga la misma información y a veces otra.

Los datos coinciden con el diseño: el **100%** de los agentes tiene un nombre on-chain, y más del **84,1%** tiene un perfil URI parseado.

**La realidad.** Como en el análisis anterior, hay que entrar a los datos para no quedarse en el diseño.

1. **El registry es un directorio, no un catálogo.** Un nombre on-chain para 441.794 agentes suena a transparencia. En detalle, más de la mitad de esos nombres no tiene una description lo bastante larga para leer, y **ningún** perfil chain en este índice lleva services, skills o tags.

   Si al interactuar con un agente solo se mira el registro on-chain, 1 de cada 2 probablemente quede catalogado como “dummy” o se descarte por información insuficiente. El resultado empeora si midéramos la calidad de la información de ese ~50% que sí registra lo suficiente: eso queda para un próximo análisis, pero el avance es que la calidad de la metadata es de alarma.

   *Si te quedás en el NFT, aprendés que el agente existe. No aprendés qué hace.*

2. **`agentURI` es un puntero, para lo bueno y para lo malo.** Un puntero off-chain da una libertad que el contrato no tiene. También carga responsabilidad sobre quien lo expone: hay que mantenerlo y poder trazar que la información siga disponible y actualizada.

   Eso ya se nota. Aproximadamente el **88,6%** de los agentes declara un puntero externo. El **79,7%** tiene en ese archivo nombre + description usables. Si pedimos también image, baja al **69,3%**. Y cuando preguntamos si el agente declara lo que hace (`services`), solo el **41,0%** lo hace.

   *Un gran poder conlleva una gran responsabilidad.*

3. **La cultura de cada chain hace más trabajo que la spec.** Celo escribe una card on-chain usable para el **90,1%** de sus agentes. X Layer lo hace para el **0,3%** y aun así setea URI en el **97%**. Ethereum L1, que abrió el año como única venue, ahora tiene la cobertura dual más débil: **21,2%** usable on-chain y **40,4%** con perfil URI. BNB, que sostiene el **67%** del stock, mantiene el corte que el estándar imagina: NFT a medias, archivo URI casi completo.

   En un mismo ecosistema ERC-8004, los hábitos de cada chain —y en muchos casos los requerimientos de sus marketplaces principales— se ven en cómo se registran los agentes locales.

   *Copiamos los hábitos de quienes seguimos.*

## Conclusión

La identidad on-chain todavía no está lo bastante madura como para, por sí sola, permitir conocer al agente con el que se quiere interactuar.

Eso crea un problema de discoverabilidad: un agente que quiera conocer a otro solo con lo registrado on-chain se encuentra con inconvenientes serios, y termina necesitando herramientas que lean off-chain por el `agentURI`.

También empuja a que los marketplaces de agentes incorporen, en su registro interno, procesos para acceder al `agentURI` y complementar servicios, de modo que los agentes puedan interactuar entre sí.

Y un tercer hecho: cualquier herramienta que quiera analizar o verificar agentes tiene que leer off-chain para complementar lo que cada agente dice poseer, y mantener procesos extra que certifiquen que esa información siga actualizada y accesible.

Si sumamos el análisis de distribución y crecimiento de ERC-8004, el estándar hasta ahora recorrió varias etapas: primero escaló como superficie de mint, después como superficie de reputation, y como **superficie de documentación que vive off-chain**. Quien lea solo el registry subcontará lo que los agentes dicen ser. Quien lea “441k agentes” como 441k cards completas sobrecontará lo que la chain sabe por sí misma.

## Qué no muestra

- Que un perfil parseado esté vivo, sea honesto o sea único. Presente no es reachable, y tampoco es un score de calidad.
- Que el 15,1% “sin card” no exista: todos tienen un nombre on-chain. Lo que falta es una description usable o un perfil URI/feedback parseado.
- Clones, templates o richness. Eso es otra lectura, no esta.
- Validation. El contrato no está en mainnet.
- Todos los deploys ERC-8004: solo las ocho mainnets de este índice.

## Cómo se cortaron estos números

Snapshot del **24 de agosto de 2026**, ~04:08 UTC. Identity en mainnet, ocho chains EVM: Ethereum L1, Base, Polygon, BNB Chain, Arbitrum, Celo, Gnosis y X Layer. Testnets afuera.

Una “card usable” es nombre no vacío **y** description de al menos 10 caracteres. Los orígenes se cuentan por fila de perfil (`chain`, URI parseada, feedback), no por una vista fusionada. Un documento almacenado detrás del puntero significa que se guardaron bytes; no es un chequeo de liveness.

Lectura relacionada, no republicada aquí: ocho meses de ERC-8004 en mainnet.

Datos indexados por Global Score Agent. Esta es una lectura de documentos de identidad ERC-8004, no un scorecard de producto.
