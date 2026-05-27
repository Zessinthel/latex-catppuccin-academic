# latex-catppuccin-academic

Template de LaTeX para documentación académica de doctorado con el sistema
de color Catppuccin. Diseñado para producir notas de clase, apuntes,
papers y reportes en **Computer and Information Science, Computational
Physics, Mathematics y Machine Learning**.

---

## Para la IA que lee este README

Este documento es tu guía operativa. El template tiene reglas estrictas
que debes internalizar antes de generar cualquier contenido. Los errores
más frecuentes de AI en este template son predecibles y evitables — todos
están documentados aquí. Lee este README completo antes de escribir una
sola línea de LaTeX.

**Pipeline de trabajo habitual:**
1. El usuario comparte este README y el contexto de la sesión.
2. El usuario proporciona notas, scripts, datos o ideas en bruto.
3. La IA formatea el contenido en el template, generando entornos,
   ecuaciones con coloreado semántico, figuras TikZ, algoritmos,
   tablas y código fuente.
4. El usuario revisa y corrige. La IA itera.

---

## Arquitectura del proyecto

```
.
├── main.tex                    ← punto de entrada; orquesta todos los \input
├── metadata.tex                ← variables del documento (\docTitulo, \docAutor, ...)
├── config/
│   ├── packages.tex            ← ÚNICA fuente de carga de paquetes
│   ├── geometry.tex            ← márgenes, encabezados, espaciado
│   ├── hyperref.tex            ← hipervínculos y metadatos PDF
│   ├── tcolorbox.tex           ← estilo base (basebox, boxfig, boxtab)
│   ├── titles.tex              ← jerarquía tipográfica de títulos
│   ├── captions.tex            ← captions de flotantes por tipo
│   ├── tables.tex              ← tablas: tipos de columna, entornos, colores
│   ├── listings.tex            ← syntax highlighting (10 lenguajes)
│   ├── algorithms.tex          ← estilo de algoritmos (algorithm2e)
│   ├── equations.tex           ← macros de coloreado semántico de ecuaciones
│   ├── pgfplots.tex            ← estilos, cycle list, colormaps
│   └── notation.tex            ← vocabulario matemático del documento
├── environments/
│   ├── generator.tex           ← macro \crearEntorno
│   ├── instances.tex           ← los 8 entornos teoremáticos
│   └── specials.tex            ← \dificultad, notasbib, ejercicio
├── themes/
│   └── catppuccin-palette.tex  ← ÚNICA fuente de verdad de color
├── frontmatter/                ← titlepage, preliminary, preface
├── chapters/                   ← un archivo .tex por capítulo
├── backmatter/                 ← apéndices
├── references/
│   ├── references.bib          ← bibliografía global
│   └── chapterN.bib            ← bibliografía por capítulo
└── code/                       ← scripts externos para \lstinputlisting
```

---

## Reglas absolutas

### REGLA I — Sistema de color: las tres capas

El sistema de color opera en tres niveles. **Nunca los mezcles.**

| Nivel | Qué contiene | Dónde vive |
|-------|-------------|------------|
| 0 — `Ctp*` | Valores hexadecimales del flavor (26 colores absolutos) | **SOLO** en `themes/catppuccin-palette.tex` |
| 1 — Estructural | Roles de UI: `background`, `surface0`, `maintext`, `overlay0` | Cualquier archivo de config |
| 2 — Semántico | Roles de dominio: `env-def`, `doc-chapter`, `code-keyword`, `tikz-accept` | Cualquier archivo de config o content |

**NUNCA** escribas `CtpBlue`, `CtpGreen`, `CtpMauve`, etc. fuera de
`catppuccin-palette.tex`. Si ves `Ctp*` en código existente del usuario,
es un error previo — corrígelo al alias semántico correspondiente.

```latex
% ❌ INCORRECTO — en cualquier archivo que no sea catppuccin-palette.tex
\draw[color=CtpBlue] ...
state/.style={fill=CtpBase, text=CtpText}

% ✅ CORRECTO — aliases semánticos
\draw[color=tikz-node-border] ...
state/.style={fill=background, text=maintext}
```

**SIEMPRE** pregunta: "¿existe ya un alias para este rol?" Busca en
`catppuccin-palette.tex` por prefijo de dominio (`env-`, `tikz-`,
`code-`, `plot-`, etc.). Si no existe, añádelo en la paleta — nunca
uses `Ctp*` directamente en el archivo de destino.

---

### REGLA II — Coloreado semántico de ecuaciones

**SIEMPRE** aplica coloreado semántico en **todas** las ecuaciones del
documento. No existe ecuación no-trivial que no tenga al menos un símbolo
con rol identificable.

**REGLA CRÍTICA: las macros de ecuación SOLO funcionan dentro de modo
matemático.** Fuera de `$...$` o `\[...\]`, el color se aplica sobre
el glifo de texto incorrecto o no se renderiza.

```latex
% ❌ INCORRECTO — fuera de modo matemático
\eqop{\nabla} f(x)          % el color no aplica

% ✅ CORRECTO — siempre dentro de $...$
$\eqop{\nabla} \equ{f}(\eqk{x})$

% ✅ En entornos matemáticos
\[
  \eqop{i}\eqcon{\hbar}\,\eqop{\partial_t}\equ{\Psi}
  = \eqop{\hat{H}}\equ{\Psi}
\]
```

**Los tres ejes de coloreado:**

| Macro | Color | Rol | Ejemplos |
|-------|-------|-----|---------|
| `\eqop{·}` | 🔵 Blue | Operador | `\eqop{\nabla}`, `\eqop{\partial}`, `\eqop{\sum}`, `\eqop{\delta}` |
| `\eqfn{·}` | 🟢 Green | Función/mapa | `\eqfn{f}(\cdot)`, `\eqfn{\mathcal{L}}`, `\eqfn{\sigma}` |
| `\eqdm{·}` | 🩵 Teal | Diferencial | `\eqdm{dx}`, `\eqdm{dt}`, `\eqdm{dW}` |
| `\eqdom{·}` | 🔹 Sapphire | Dominio/conjunto | `\eqdom{\Omega}`, `\eqdom{\mathbb{R}^n}`, `\eqdom{\Sigma}` |
| `\equ{·}` | 🍑 Peach | Incógnita | `\equ{u}`, `\equ{\theta}`, `\equ{\psi}` |
| `\eqk{·}` | 💜 Mauve | Dato/parámetro | `\eqk{\lambda}`, `\eqk{m}`, `\eqk{q_0}` |
| `\eqcon{·}` | 🪻 Lavender | Constante universal | `\eqcon{\hbar}`, `\eqcon{k_B}`, `\eqcon{\pi}` |
| `\eqvec{·}` | 🔴 Red | Vector/campo | `\eqvec{\mathbf{E}}`, `\eqvec{\mathbf{u}}` |
| `\eqten{·}` | 🌸 Rosewater | Tensor/matriz | `\eqten{\sigma_{ij}}`, `\eqten{W}` |

**Aplica este coloreado en TODOS los contextos:**
- Ecuaciones inline: `$\eqop{\nabla}\equ{u}$`
- Ecuaciones display: `\[...\]`, `equation`, `align`
- Dentro de entornos tcolorbox (definicion, teorema, etc.)
- Celdas de tablas: `$\eqop{\delta}: \eqdom{Q} \eqop{\times} \eqdom{\Sigma} \to \eqdom{Q}$`
- Labels de nodos TikZ: `node {$\eqk{a}$}`
- Dentro de `algorithm2e`: `$\equ{U} \gets \eqop{E}(\eqop{move}(\equ{T}, \equ{a}))$`

**Macros compuestos disponibles:**
```latex
\eqpdv{f}{t}          % derivada parcial coloreada
\eqgrad{u}            % gradiente coloreado
\eqdvg{\mathbf{v}}    % divergencia (evita colisión con \div de physics)
\eqSDE{X}{f(X)}{\sigma}  % SDE completa coloreada
```

**Toggle para impresión B/N:**
```latex
\eqcolorsfalse   % desactiva todo el coloreado — macros transparentes
\eqcolorstrue    % reactiva (por defecto)
```

---

### REGLA III — Selección de entornos

**NO abuses de los entornos.** Cada uno tiene un rol epistémico preciso.
Usarlos fuera de ese rol degrada el argumento matemático.

| Entorno | Color | Úsalo cuando... | NO lo uses para... |
|---------|-------|----------------|--------------------|
| `definicion` | Blue | Estableces vocabulario formal por primera vez | Enunciados que ya se conocen |
| `teorema` | Mauve | El resultado principal al que apunta el argumento | Resultados secundarios o de apoyo |
| `proposicion` | Green | Resultado correcto pero subordinado al teorema | El resultado más importante del capítulo |
| `ejemplo` | Peach | Instancias concretas que iluminan lo abstracto | Demostraciones o definiciones |
| `observacion` | Gray | Matices, advertencias, notas de implementación | Resultados matemáticos con demostración |
| `fisicaconexion` | Sapphire | Conexión explícita entre matemática y física | Notas de diseño de software |
| `estrella` | Yellow | El concepto pivote que orienta todo el capítulo | Como mucho 1 por capítulo |
| `ejerciciocomp` | Pink | Tareas que requieren código, simulación, datos | Ejercicios matemáticos puros |

**Sintaxis:**
```latex
\begin{definicion}{Título descriptivo de esta instancia}
\label{def:etiqueta}
Contenido...
\end{definicion}
```

**Indicador de dificultad** — colocar al inicio de secciones densas:
```latex
\dificultad{3}{4}   % matemática: ★★★☆☆   física: ★★★★☆
```

---

### REGLA IV — Referencias bibliográficas

**SIEMPRE** incluye `\cite` en cada afirmación no trivial. Si no conoces
la referencia correcta, **pregunta al usuario** — nunca inventes una cita.

**Estructura de bibliografía por capítulo:**
```latex
% Cada capítulo tiene su propio .bib en references/
% Registrado en metadata.tex:
\addbibresource{references/chapterN.bib}

% Al final de cada capítulo .tex:
\printbibliography[heading=subbibliography]
```

**Cuándo pedir referencias al usuario:**
- Definiciones formales tomadas de un libro
- Teoremas con nombre propio (Riesz, Fokker-Planck, Bayes)
- Algoritmos con autor conocido (Dijkstra, Euler-Maruyama, Thompson)
- Afirmaciones sobre complejidad computacional
- Resultados experimentales o numéricos

```latex
% ✅ Correcto — citar la fuente
\begin{definicion}{Espacio de Hilbert}
Un espacio de Hilbert es...~\cite{reed1980}
\end{definicion}

% ❌ Incorrecto — afirmación sin fuente
\begin{teorema}{Completitud de $L^2$}
El espacio $L^2$ es completo.
\end{teorema}
```

---

### REGLA V — Flotantes y tcolorbox

`figure` y `table` son flotantes que invocan `\@float` — incompatible
con el interior de `tcolorbox`. El error es `Not in outer par mode`.

| Contexto | Figuras | Tablas | Ancho |
|----------|---------|--------|-------|
| Flujo normal de texto | `figure` | `acadtable` | `\textwidth` |
| Dentro de tcolorbox | `boxfig` | `boxtab` | `\linewidth` |

```latex
% ✅ Figura dentro de un entorno
\begin{ejemplo}{Distribución gaussiana}
  \begin{boxfig}{fig:gauss}{Densidad $\mathcal{N}(0,1)$}
    \includegraphics[width=\linewidth]{figs/gauss.pdf}
  \end{boxfig}
\end{ejemplo}

% ✅ Tabla dentro de un entorno
\begin{definicion}{Propiedades}
  \begin{boxtab}{tab:props}{Resumen}
    \begin{tabularx}{\linewidth}{l X}   % ← linewidth, no textwidth
      ...
    \end{tabularx}
  \end{boxtab}
\end{definicion}
```

**`\label` dentro de tcolorbox:** siempre *dentro* del entorno,
nunca en el argumento del título:
```latex
\begin{definicion}{Espacio métrico}
\label{def:espacio-metrico}   % ← aquí, no en {Espacio métrico}
...
```

---

### REGLA VI — Diagramas TikZ con coloreado correcto

**NUNCA** uses `CtpBlue`, `CtpBase`, `CtpText` en nodos TikZ.

```latex
% ❌ INCORRECTO — viola la arquitectura de color
state/.style={circle, draw=CtpBlue, fill=CtpBase, text=CtpText}

% ✅ CORRECTO — aliases semánticos
state/.style={circle, draw=tikz-node-border, fill=background, text=maintext}
```

**Aliases TikZ disponibles:**

| Alias | Uso |
|-------|-----|
| `tikz-node-bg` | Fondo de nodo genérico |
| `tikz-node-border` | Borde de nodo genérico |
| `tikz-edge` | Arista estándar |
| `tikz-label` | Label / anotación |
| `tikz-initial` | Estado inicial (autómata) |
| `tikz-accept` | Estado aceptador |
| `tikz-reject` | Estado rechazador |
| `tikz-highlight` | Nodo activo en explicación paso a paso |
| `env-phys` | Caja de función / transformación (Sapphire) |
| `fb-error` | Colisión / error |
| `eq-known` | Peso de arista (dato conocido) |

**Patrón canónico para autómatas:**
```latex
\begin{tikzpicture}[
  shorten >=1pt, node distance=2.5cm, on grid, auto, thick,
  state/.style={circle, draw=tikz-node-border, fill=background,
                text=maintext, minimum size=1cm},
  every edge/.style={draw=tikz-edge, ->, >=latex},
]
  \node[state, initial, initial text={}, draw=tikz-initial] (q0) {$\eqk{q_0}$};
  \node[state, accepting, draw=tikz-accept, right=of q0]    (q1) {$\eqk{q_1}$};
  \path[->]
    (q0) edge node {$\eqk{a}$} (q1);
\end{tikzpicture}
```

**Labels de nodos y aristas:** siempre en modo matemático con coloreado semántico:
```latex
edge node {$\eqk{a}$}           % ← símbolo conocido del alfabeto
node {$\eqcon{\varepsilon}$}    % ← constante epsilon
node[above] {$\eqk{w}$}        % ← peso conocido
```

---

### REGLA VII — Algoritmos (algorithm2e)

`algorithm2e` **no** está envuelto en tcolorbox — rompe el paginado.
Úsalo directamente, sin wrapper. Aplica coloreado semántico de ecuaciones
en todas las expresiones matemáticas del pseudocódigo:

```latex
\begin{algorithm}[H]
  \DontPrintSemicolon
  \KwIn{$\eqk{A} \in \eqdom{\mathbb{R}^{n \times n}}$ SPD, $\eqk{b}$}
  \KwOut{$\equ{x} \approx \eqk{A}^{-1}\eqk{b}$}
  $\equ{x} \gets \mathbf{0}$\;
  \While{$\|\equ{r}\| > \eqk{\epsilon}$}{
    $\equ{\alpha} \gets \equ{\rho} / (\equ{p}^\top \eqk{A}\equ{p})$\;
    $\equ{x} \gets \equ{x} + \equ{\alpha}\equ{p}$\;
  }
  \caption{Gradiente conjugado}
  \label{alg:cg}
\end{algorithm}
```

---

### REGLA VIII — Código fuente (listings)

**Siempre** especifica el estilo del lenguaje. Para código LaTeX
(cuando muestras ejemplos del template), usa `style=latex`:

```latex
\begin{lstlisting}[style=python, caption={Descripción}, label={lst:etiqueta}]
...
\end{lstlisting}
```

Estilos disponibles: `python`, `c`, `cpp`, `rust`, `go`, `julia`,
`matlab`, `fortran`, `asm`, `mathematica`, `latex`.

Para importar archivos externos:
```latex
\lstinputlisting[style=python, caption={...}, label={lst:...}]{code/archivo.py}
% Fragmento: solo líneas 15-40
\lstinputlisting[style=python, firstline=15, lastline=40, ...]{code/archivo.py}
```

---

## Árbol de decisión: ¿qué entorno uso?

```
¿Qué tipo de contenido es?
│
├── Vocabulario nuevo que se usará formalmente    → definicion
├── Resultado principal demostrado/demostrable
│   ├── Es el resultado central del capítulo      → teorema
│   └── Es un resultado de apoyo                  → proposicion
├── Caso concreto que ilustra algo abstracto      → ejemplo
├── Nota, matiz, advertencia técnica              → observacion
├── Conexión explícita con física real            → fisicaconexion
├── El concepto más importante del capítulo
│   (máx. 1 por capítulo)                         → estrella
├── Tarea que requiere código/simulación           → ejerciciocomp
├── Notas de lectura / contexto bibliográfico     → notasbib
└── Ejercicio matemático sin código               → ejercicio (comando)
```

---

## Árbol de decisión: ¿qué color de ecuación uso?

```
¿Cuál es el rol del símbolo?
│
├── ¿Es un operador? (∇, ∂, ∑, ∫, δ, ×, ∘)       → \eqop{·}
├── ¿Es una función o mapa? (f(·), σ(·), L)        → \eqfn{·}
├── ¿Es un diferencial o medida? (dx, dt, dW, dμ)  → \eqdm{·}
├── ¿Es un dominio o conjunto? (Ω, Σ, ℝⁿ, Q)      → \eqdom{·}
│
├── ¿Es la cantidad buscada / aprendida?            → \equ{·}
├── ¿Es un dato fijo / parámetro conocido?          → \eqk{·}
├── ¿Es una constante universal? (ℏ, kB, π, c)     → \eqcon{·}
│
├── ¿Es un vector o campo? (u, E, B, J)             → \eqvec{·}
└── ¿Es un tensor o matriz? (σᵢⱼ, gμν, W, Σ)     → \eqten{·}
```

---

## Mapa de aliases de color por dominio

```latex
% Estructura del documento
doc-chapter  doc-section  doc-subsection  doc-subsubsection

% Hipervínculos
link-internal  link-external  link-cite  link-visited

% Entornos
env-def  env-thm  env-prop  env-ex  env-obs  env-phys  env-star  env-comp

% Ecuaciones
eq-op  eq-fn  eq-dm  eq-dom     % eje sintáctico
eq-unknown  eq-known  eq-const  % eje epistémico
eq-vec  eq-ten                  % eje de tipo

% Código
code-keyword  code-string  code-comment  code-operator  code-type
code-fn  code-mod  code-macro  code-builtin  code-const  code-param

% Algoritmos
alg-keyword  alg-arg  alg-comment  alg-linenum  alg-caption  alg-line

% Plots
plot-series-1..8  plot-axis  plot-grid  plot-label  plot-tick-label
plot-legend-bg  plot-legend-border  plot-errorbar

% TikZ
tikz-node-bg  tikz-node-border  tikz-edge  tikz-label
tikz-initial  tikz-accept  tikz-reject  tikz-highlight

% Tablas
table-head  table-head-text  table-row-alt  table-rule

% Captions
caption-fig  caption-tab  caption-code  caption-alg  caption-sub  caption-text

% Feedback
fb-success  fb-warning  fb-error  fb-info  fb-difficulty

% Infraestructura
background  surface0/1/2  overlay0/1/2  maintext  subtext0/1
```

---

## Errores comunes de IA en este template

### ❌ Error 1: replicar `Ctp*` del código existente
Si el usuario te muestra código con `CtpBlue` o `fill=CtpGreen`, eso es
un error en el código fuente. No lo repliques — corrígelo.

### ❌ Error 2: ecuaciones fuera de modo matemático
`\eqop{\nabla}` en prosa no colorea nada. Siempre `$\eqop{\nabla}$`.

### ❌ Error 3: usar `teorema` para todo lo que parece importante
`teorema` es para el resultado principal. Una observación interesante
es `observacion`. Un hecho auxiliar es `proposicion`.

### ❌ Error 4: inventar referencias bibliográficas
Si no sabes la cita correcta, escribe `\cite{???}` como placeholder
y pide la referencia al usuario. Nunca inventes autores ni títulos.

### ❌ Error 5: `\textwidth` dentro de tcolorbox
Dentro de `boxtab`, `boxfig` o cualquier entorno tcolorbox, el ancho
correcto es `\linewidth`. Con `\textwidth` la tabla o imagen desborda.

### ❌ Error 6: `figure`/`table` dentro de tcolorbox
Produce `Not in outer par mode`. Usar `boxfig`/`boxtab`.

### ❌ Error 7: `\label` fuera del entorno
```latex
% ❌ El anchor de hyperref queda mal
\begin{definicion}{Título}\label{def:x}\end{definicion}
% ✅ Correcto
\begin{definicion}{Título}
\label{def:x}
...
\end{definicion}
```

### ❌ Error 8: headers de columna `S` de siunitx sin `\multicolumn`
Cualquier header de columna `S` que contenga texto (incluyendo `Acc. (%)`
o `$\|\mathbf{e}\|_\infty$`) debe ir en `\multicolumn{1}{c}{\thead{...}}`.

---

## Vocabulario de notación matemática disponible

Definido en `config/notation.tex`. Úsalo en lugar de redefinir:

```latex
% Conjuntos numéricos
\R \N \Z \C \Q \F

% Álgebra lineal
\T          % transpuesta: A\T
\HH         % adjunto: A\HH
\Id         % identidad
\diag \spec \argmin \argmax

% Probabilidad y estadística
\E{X}       % E[X]
\Prob{A}    % P(A)
\Ep{f}      % ⟨f⟩
\KL \Var \Cov
\Normal \Uniform \Bernoulli \Poisson

% Machine Learning
\Loss       % ℒ (cal)
\Lscr       % ℒ (script)
\D          % 𝒟 dataset
\Params     % Θ
\params     % θ
\score      % ∇_x log p  (arg opcional: \score[\mathbf{z}])
\SDE{X}{a}{b}   % dX = a dt + b dW (sin color, para inline)

% Física
\Ham        % Ĥ
\Hscr       % ℋ (Hilbert)
\Fscr       % ℱ (Fourier)
\kB         % k_B
\keff       % k_eff
\FP         % ℒ_FP (Fokker-Planck)
```

---

## Protocolo de extensión (dominio nuevo)

```latex
% 1. En themes/catppuccin-palette.tex:
% ─── {Dominio} ───────────────────────────────────────────────────────────────
\colorlet{dom-role1}{CtpAlgo}
\colorlet{dom-role2}{CtpOtro}
\createback{dom-role1}   % si se usa como fondo en tcolorbox

% 2. Crear config/{dominio}.tex consumiendo SOLO aliases (nunca Ctp*):
\tikzset{
  mi-estilo/.style={draw=dom-role1, fill=dom-role2},
}

% 3. En main.tex, paso 8 (configs de dominio):
\input{config/{dominio}.tex}
```

---

## Contexto de dominio

El contenido producido pertenece a los siguientes campos. Usa esta
información para elegir la notación correcta y las referencias apropiadas:

- **Computer and Information Science**: complejidad computacional, estructuras
  de datos, compiladores, teoría de lenguajes formales, autómatas,
  algoritmos, análisis de algoritmos.

- **Computational Physics**: métodos numéricos (diferencias finitas,
  elementos finitos, Monte Carlo), ecuaciones diferenciales parciales
  (Poisson, calor, onda, Schrödinger), mecánica estadística, procesos
  estocásticos, simulación de Monte Carlo.

- **Mathematics**: análisis funcional, espacios de Hilbert y Banach,
  operadores, álgebra abstracta, topología, teoría de medida, ecuaciones
  diferenciales estocásticas, procesos de Markov.

- **Machine Learning**: aprendizaje profundo, redes neuronales, métodos
  variacionales (ELBO, VAE), modelos de difusión, descenso de gradiente,
  optimización estocástica, inferencia bayesiana.

---

## Referencias de implementación

La documentación completa del template vive en `chapters/` y `backmatter/`:

- **Apéndice A** — Style Guide: arquitectura, árbol de decisión, inventario
  de aliases, protocolo de extensión.
- **Apéndice C** — Referencia de comandos: firma completa de todos los
  comandos, entornos y macros del template.
- **Capítulo 4** — Matemáticas semánticas: ejemplos detallados del
  coloreado de ecuaciones en todos los contextos.
- **Capítulo 8** — Diagramas: patrones TikZ con theming correcto.

---

*Motor requerido: LuaLaTeX. Distribución: TeX Live 2022 o superior.*
*Compilación: `lualatex → biber → lualatex × 2`*
