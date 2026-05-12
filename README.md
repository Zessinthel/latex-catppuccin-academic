# 📘 Template LaTeX para Libros Técnicos (Física, Matemáticas, ML, Computación)

Documentación completa del template `latex-catppuccin-academic`. Esta guía describe cada comando, entorno, opción de color y convención para que una inteligencia artificial (o un humano) pueda generar archivos `.tex` completamente compatibles.

---

## 1. Propósito y alcance

Este template permite escribir libros estructurados (tesis, notas de clase, apuntes avanzados) con:

- **Sintaxis semántica rica**: comandos para conjuntos, operadores, estatus epistémico.
- **Entornos tipo `tcolorbox`**: definiciones, teoremas, ejemplos, ejercicios, etc.
- **Sistema de coloreado semántico de ecuaciones** (tres ejes: rol sintáctico, estatus epistémico, tipo matemático).
- **Paleta Catppuccin** completa con soporte claro/oscuro (Latte, Frappé, Macchiato, Mocha).
- **Listings multilingüe** (10 lenguajes) con colores Catppuccin.
- **Configuración modular**: cada aspecto en su propio archivo dentro de `config/`, `environments/`, `themes/`, etc.

---

## 2. Motor y dependencias

- **Compilador**: LuaLaTeX (requerido por `catppuccinpalette` y listings UTF-8).
- **Compilación típica**:
  ```bash
  lualatex main.tex
  biber main
  lualatex main.tex
  makeindex main   # si se usa índice
  lualatex main.tex
  ```
- **Paquetes críticos** (cargados en `config/packages.tex`):
  - `physics`, `amsmath`, `amssymb`, `mathtools`, `bm`, `cancel`, `stmaryrd`
  - `tcolorbox` (con librerías `theorems`, `breakable`, `skins`, `hooks`)
  - `listings`, `inconsolata`
  - `catppuccinpalette`
  - `titlesec`, `fancyhdr`
  - `hyperref`, `bookmark`, `cleveref` (configurado en español)
  - `biblatex` (backend `biber`, estilo numérico, `refsection=chapter`)
  - `pgfplots`, `tikz` (librerías varias)
  - `algorithm2e`, `siunitx`, `booktabs`, `tabularx`, `longtable`, `multirow`, `colortbl`
  - `enumitem`, `pifont`, `csquotes`, `microtype`, `footmisc`, `appendix`, `pdfpages`

---

## 3. Estructura del proyecto

```
.
├── main.tex                       # Documento principal
├── metadata.tex                   # Variables de metadatos
├── config/
│   ├── packages.tex               # Carga de todos los paquetes
│   ├── geometry.tex               # Márgenes, interlineado, encabezados
│   ├── hyperref.tex               # Configuración de enlaces y metadatos PDF
│   ├── macros.tex                 # Comandos matemáticos propios
│   ├── tcolorbox-config.tex       # Estilo base `basebox`
│   ├── titles-config.tex          # Formato y color de capítulos/secciones
│   ├── listings-catppuccin.tex    # Estilos de listings por lenguaje
│   ├── algorithms.tex             # Estilo visual de algoritmos
│   ├── tables-config.tex          # Columnas, colores, entornos de tablas
│   ├── equation-styles.tex        # Coloreado semántico de ecuaciones
│   ├── pgfplots-config.tex        # Estilos y colormaps para gráficas
│   └── captions-config.tex        # Formato de captions (figura/tabla/código)
├── environments/
│   ├── generator.tex              # Constructor `\crearEntorno`
│   ├── instances.tex              # Instancias concretas de entornos
│   └── specials.tex               # Entornos/commandos especiales (dificultad, ejercicios, etc.)
├── themes/
│   └── catppuccin-palette.tex     # Paleta completa, colores semánticos, soporte claro/oscuro
├── frontmatter/
│   ├── titlepage.tex              # Portada genérica
│   ├── preliminary.tex            # Dedicatoria, epígrafe, agradecimientos
│   └── preface.tex                # Prefacio (ejemplo)
├── chapters/
│   └── chapter1.tex               # Capítulo de demostración
├── backmatter/
│   └── appendixA.tex              # Apéndice de ejemplo (galería de código)
├── references/
│   └── references.bib             # Archivo de bibliografía
└── figs/                          # Imágenes
```

---

## 4. Metadatos del documento (`metadata.tex`)

Estas variables se definen al principio y se usan en portada, PDF y preliminares.

```latex
\newcommand{\docTitulo}{Título del documento}
\newcommand{\docSubtitulo}{Subtítulo del documento\\ con saltos de línea}
\newcommand{\docAutor}{Autor del documento}
\newcommand{\docFecha}{\the\year}               % Año actual
\newcommand{\docEstado}{Versión preliminar}      % "Borrador", "Final", etc.
\newcommand{\docCita}{``Cita representativa del documento.''}
\newcommand{\docDedicatoria}{A quienes hicieron posible este trabajo.}
\newcommand{\docEpigrafe}{``Epígrafe o cita inicial.''}
\newcommand{\docAgradecimientos}{Quiero expresar mi gratitud...}
\newcommand{\docKeywords}{palabra clave 1, palabra clave 2}
\newcommand{\docSubject}{Área temática principal}
```

El archivo de bibliografía se declara con:

```latex
\addbibresource{references/references.bib}
```

Uso en el documento: se emplean directamente (`\docTitulo`, `\docAutor`, etc.) en
`titlepage.tex`, `preliminary.tex` y en la configuración de `hyperref` para los metadatos PDF.

---

## 5. Paleta de colores y tema (`themes/catppuccin-palette.tex`)

### 5.1 Selección del sabor

En `main.tex` se carga el archivo de tema:

```latex
\input{themes/catppuccin-palette.tex}
```

Dentro de ese archivo se elige un solo sabor descomentando la línea correspondiente:

```latex
\usepackage[Latte,styleAll]{catppuccinpalette}   % Claro
%\usepackage[Frappe,styleAll]{catppuccinpalette} % Oscuro
%\usepackage[Macchiato,styleAll]{catppuccinpalette}
%\usepackage[Mocha,styleAll]{catppuccinpalette}
```

Además, se debe ajustar manualmente el toggle de tema oscuro:

```latex
\newif\ifdarktheme
\darkthemefalse   % para Latte
%\darkthemetrue   % para Frappé/Macchiato/Mocha
```

Esto controla si el fondo de página es blanco o `CtpBase`, y si los colores «light» usan
`white!97!color` (claro) o `CtpBase!95!color` (oscuro).

### 5.2 Colores semánticos principales

| Categoría | Color(es) |
|-----------|-----------|
| Fondo | CtpBase, CtpMantle, CtpCrust |
| Superficie/solapas | CtpSurface0, CtpSurface1, CtpSurface2 |
| Overlays (texto apagado) | CtpOverlay0, CtpOverlay1, CtpOverlay2 |
| Texto | CtpText, CtpSubtext0, CtpSubtext1 |
| Enlaces | linkcolor (azul), citecolor (verde), urlcolor (melocotón) |
| Entornos | defcolor (azul), thmcolor (malva), propcolor (verde), ejemcolor (melocotón), obscolor (gris), fisicacolor (zafiro), estrellacolor (amarillo), compcolor (rosa) |
| Mensajes funcionales | success (verde), warning (amarillo), error (rojo), info (turquesa) |
| Código (listings) | keywordcolor (malva), stringcolor (verde), commentcolor (gris), typecolor (amarillo), etc. |
| Títulos | chaptercolor (rosa), sectioncolor (celeste), subsectioncolor (lavanda), subsubsectioncolor (melocotón) |

**Importante**: Los colores de entornos y títulos ya están mapeados a la paleta
Catppuccin. No se deben redefinir a menos que se modifique `catppuccin-palette.tex`.

### 5.3 Versiones «light» para fondos de cajas

Se generan automáticamente con `\createLightColor{<nombre>}{<color base>}`. Por ejemplo,
`defcolorlight` es el color de fondo de las cajas de definición (muy claro en modo claro,
apenas tintado en modo oscuro). Todos los entornos definidos en `instances.tex` usan su
correspondiente `-light`.

### 5.4 Atajos de texto

```latex
\textsuccess{...}   % verde
\textwarning{...}   % amarillo
\texterror{...}     % rojo
\textinfo{...}      % turquesa
\textkeyword{...}   % malva
\textstring{...}    % verde
\textcomment{...}   % gris
```

---

## 6. Comandos matemáticos

### 6.1 Macros generales (`config/macros.tex`)

El paquete `physics` ya provee `\abs`, `\norm`, `\dv`, `\pdv`, `\grad`, `\div`, `\laplacian`, etc.

Los comandos adicionales son:

| Comando | Resultado | Uso |
|--------|-----------|-----|
| `\R`, `\N`, `\Z`, `\C`, `\Q` | ℝ, ℕ, ℤ, ℂ, ℚ | Conjuntos numéricos |
| `\T` | `^\top` | Transpuesta |
| `\argmin`, `\argmax` | arg min, arg max | Optimización |
| `\Loss` | 𝒞 | Función de pérdida |
| `\D` | 𝒟 | Dataset |
| `\Params` | Θ | Parámetros (mayúscula) |
| `\params` | θ | Parámetros (minúscula) |
| `\KL`, `\Var`, `\Cov` | KL, Var, Cov | Estadística |
| `\E{·}` | 𝔼[·] | Esperanza |
| `\Lap` | ∇² | Laplaciano (alias) |
| `\Div` | ∇· | Divergencia (alias) |
| `\keff` | k_eff | Física de reactores |
| `\score` | ∇_x log p | Score function |
| `\Ep{·}` | ⟨·⟩ | Valor esperado (física) |
| `\FP` | ℒ_FP | Fokker-Planck |
| `\SDE{X}{a}{b}` | dX = a dt + b dW | Ecuación diferencial estocástica |
| `\OK` | ✓ | Marca de verificación |

### 6.2 Coloreado semántico de ecuaciones (`config/equation-styles.tex`)

Sistema opt-in para colorear símbolos según su rol o estatus.

**Eje 1 – Rol sintáctico** (estable entre contextos):

- `\eqop{·}` → azul (operadores: ∫, ∑, ∇, ∂)
- `\eqfn{·}` → verde (funciones: f(·), σ(·))
- `\eqdm{·}` → turquesa (diferenciales: dx, dt, dW)
- `\eqdom{·}` → zafiro (dominios: Ω, ℝⁿ)

**Eje 2 – Estatus epistémico** (lo que la tipografía no puede codificar):

- `\equ{·}` → melocotón (incógnita / a determinar)
- `\eqk{·}` → malva (conocido / dato fijo)
- `\eqcon{·}` → lavanda (constante universal)

**Eje 3 – Tipo matemático** (refuerzo):

- `\eqvec{·}` → rojo (campo vectorial)
- `\eqten{·}` → rosa (tensor / matriz)

**Comandos compuestos:**

- `\eqpdv{ρ}{t}` → derivada parcial coloreada
- `\eqgrad{u}` → gradiente coloreado
- `\eqdvg{u}` → divergencia coloreada
- `\eqSDE{X}{drift}{diff}` → SDE coloreada

Toggle global: `\eqcolorstrue` (color) / `\eqcolorsfalse` (b/n).

Ejemplo:

```latex
\eqop{\frac{\partial \equ{\phi}}{\partial \eqk{t}}} = \eqcon{D} \eqop{\nabla^2} \equ{\phi}
```

---

## 7. Entornos de caja (`tcolorbox`)

### 7.1 Estilo base (`config/tcolorbox-config.tex`)

Todas las cajas comparten `basebox`: bordes finos, fondo gris muy claro,
breakable, sombra difusa.

### 7.2 Generador de entornos (`environments/generator.tex`)

```latex
\crearEntorno{nombre}{color}{título base}{contador}{opciones}
```

Si `#4=1`: crea un contador `nombrecounter` (numeración capítulo.contador).

El entorno resultante se llama con `\begin{nombre}{Título} ... \end{nombre}`.

### 7.3 Entornos instanciados (`environments/instances.tex`)

| Entorno | Color | Título base | Contador | Notas |
|---------|-------|-------------|----------|-------|
| `definicion` | defcolor | Definición | Sí | |
| `teorema` | thmcolor | Teorema | Sí | |
| `proposicion` | propcolor | Proposición | Sí | |
| `ejemplo` | ejemcolor | Ejemplo | Sí | |
| `observacion` | obscolor | Observación | No | |
| `fisicaconexion` | fisicacolor | [F] Conexión física | No | Texto pequeño (`\small`) |
| `estrella` | estrellacolor | ★ Estrella Polar | Sí | Texto pequeño |
| `ejerciciocomp` | compcolor | ▷ Ejercicio Computacional | Sí | Texto pequeño |

Sintaxis exacta (para todos):

```latex
\begin{definicion}{Espacio de Hilbert}
...
\end{definicion}
```

### 7.4 Entornos y comandos especiales (`environments/specials.tex`)

#### `\dificultad{mat}{fis}`

Muestra estrellas de 1 a 5 para dificultad matemática y física.

```latex
\dificultad{3}{2}
```

Resultado: **Nivel: Matemática: ★★★☆☆ Física: ★★☆☆☆**

#### `notasbib`

Entorno para «Notas bibliográficas» al final del capítulo.

```latex
\begin{notasbib}
\item Para el teorema 2.1, ver \cite{...}.
\end{notasbib}
```

#### `ejercicio`

Ejercicio simple (sin caja, numerado por capítulo).

```latex
\begin{ejercicio}
Demuestre que...
\end{ejercicio}
```

Se renderiza como **Ejercicio X.Y.** seguido del texto.

---

## 8. Listings multilingüe (`config/listings-catppuccin.tex`)

Estilos disponibles (alias entre paréntesis):

| Alias | Lenguaje |
|-------|----------|
| `python` | Python |
| `c` | C |
| `cpp` | C++ |
| `rust` | Rust |
| `go` | Go |
| `julia` | Julia |
| `matlab` | MATLAB/Octave |
| `fortran` | Fortran (2008) |
| `asm` | x86-64 Assembly |
| `mathematica` | Wolfram Language |

Uso:

```latex
\begin{lstlisting}[style=python, caption={Descripción}]
...
\end{lstlisting}
```

O para archivos externos:

```latex
\lstinputlisting[style=rust]{codigo/main.rs}
```

---

## 9. Formato de títulos (`config/titles-config.tex`)

- **Capítulo**: `\Huge\bfseries\color{chaptercolor}` (rosa)
- **Sección**: `\Large\bfseries\color{sectioncolor}` (celeste)
- **Subsección**: `\large\bfseries\color{subsectioncolor}` (lavanda)
- **Subsubsección**: `\normalsize\bfseries\color{subsubsectioncolor}` (melocotón)

Los colores se definen en `catppuccin-palette.tex`.

---

## 10. Tablas (`config/tables-config.tex`)

### 10.1 Tipos de columna

- `L{ancho}`, `C{ancho}`, `R{ancho}`: columnas de ancho fijo con wrapping y alineación vertical centrada (`m{}`).
- `X` (redefinida): columna proporcional izquierda con centrado vertical.
- `Y`, `Z`: columnas proporcionales centrada y derecha respectivamente.
- `S{...}`: columna de `siunitx` para alineación decimal.

### 10.2 Colores y comandos

- `\thead{...}`: fuente negrita + color de texto del encabezado.
- `\headerrow`: aplica `\rowcolor{tableheadcolor}` (fondo azul claro).
- `\tablenote{}{...}`: nota al pie de tabla.
- `\arrayrulecolor{tablerule}` se aplica automáticamente.

### 10.3 Entornos útiles

#### `fittable`

Reescala la tabla solo si excede `\textwidth`.

```latex
\begin{fittable}
  \begin{tabular}{...} ... \end{tabular}
\end{fittable}
```

#### `acadtable`

Crea un `table` flotante con caption y etiqueta.

```latex
\begin{acadtable}[htbp]{tab:mi_tabla}{Título de la tabla}
  \begin{tabularx}{\textwidth}{l Y Y}
    ...
  \end{tabularx}
\end{acadtable}
```

---

## 11. Algoritmos (`config/algorithms.tex`)

Basado en `algorithm2e` con líneas coloreadas. Los keywords se muestran en malva,
comentarios en gris, números en celeste.

Sintaxis típica:

```latex
\begin{algorithm}[H]
\caption{Mi algoritmo}
\label{alg:algo}
\LinesNumbered
\KwIn{Entrada}
\KwOut{Salida}
... \\
\While{condición}{
  ... \\
}
\Return resultado\;
\end{algorithm}
```

---

## 12. Gráficas con pgfplots (`config/pgfplots-config.tex`)

**Cycle list**: 8 colores Catppuccin automáticos para `\addplot`.

**Estilos predefinidos**:
- `catppuccin-clean`: grid mayor, leyenda estándar.
- `catppuccin-filled`: igual pero con fondo `CtpMantle`.

**Colormaps**:
- `catppuccin-heat`: secuencial (oscuro → azul → rojo → amarillo).
- `catppuccin-diverge`: divergente (azul → neutro → rojo).

Ejemplo:

```latex
\begin{tikzpicture}
  \begin{axis}[catppuccin-clean, width=0.8\textwidth, height=6cm,
               xlabel={Época}, ylabel={Pérdida}]
    \addplot+[thick, no markers, domain=0:100] {2*exp(-0.05*x)};
    \addlegendentry{Adam}
  \end{axis}
\end{tikzpicture}
```

---

## 13. Captions (`config/captions-config.tex`)

Formato general: fuente `small`, etiqueta en negrita coloreada, texto en `CtpSubtext0`,
separador endash.

- Figuras: etiqueta azul.
- Tablas: etiqueta malva, posición `above`.
- Listings: etiqueta verde.
- Subfiguras: etiqueta zafiro.

Las leyendas se escriben con `\caption{...}` dentro del flotante.

---

## 14. Hipervínculos y metadatos PDF (`config/hyperref.tex`)

Colores: `linkcolor` para referencias internas, `citecolor` para citas, `urlcolor` para URLs.
Metadatos del PDF: título, autor, tema, palabras clave.

---

## 15. Capítulos y estructura del contenido

### 15.1 Esqueleto de un capítulo

```latex
\chapter{Título del capítulo}
\label{ch:mi_capitulo}

\section{Sección 1}
...

\begin{definicion}{...}
...
\end{definicion}

\begin{teorema}{...}
...
\end{teorema}

\begin{ejemplo}{...}
...
\end{ejemplo}

\begin{ejercicio}
...
\end{ejercicio}
```

### 15.2 Frontmatter

- `titlepage.tex`: usa `\docTitulo`, `\docSubtitulo`, etc. y una caja para la cita.
- `preliminary.tex`: muestra dedicatoria, epígrafe y agradecimientos si no están vacíos.
- `preface.tex`: capítulo sin numerar agregado al TOC.

### 15.3 Apéndices

Se incluyen con:

```latex
\begin{appendices}
\include{backmatter/appendixA}
\end{appendices}
```

Cada apéndice es un capítulo normal (`\chapter{...}`) dentro del entorno `appendices`.
Las referencias a apéndices usan `\cref{ap:...}`.

---

## 16. Bibliografía (`references/references.bib`)

Archivo BibTeX estándar. Se carga con `\addbibresource{references/references.bib}`.
Para mostrar la bibliografía al final de cada capítulo (debido a `refsection=chapter`):

```latex
\printbibliography[heading=subbibliography]
```

Las citas se hacen con `\cite{clave}`, `\citet{clave}`, `\citep{clave}` (compatibles con `natbib`).

---

## 17. Convenciones para generación por IA

Al generar contenido `.tex` para este template, observar estrictamente:

- **Entornos**: siempre usar `\begin{entorno}{Título} ... \end{entorno}`.
- **Ecuaciones coloreadas**: envolver símbolos con los macros `\eqop`, `\equ`, `\eqk`, etc.
- **Colores**: no usar colores explícitos, confiar en los definidos en la paleta.
- **Tablas**: preferir `acadtable` + `tabularx` con columnas `Y`, `Z`, `L`, `C`, `R`.
- **Algoritmos**: usar `algorithm2e` como en los ejemplos.
- **Listings**: usar `style=<alias>`.
- **Ejercicios**: teóricos con `ejercicio`, computacionales con `ejerciciocomp`.
- **Dificultad**: `\dificultad{mat}{fis}`.
- **Bibliografía**: `\cite{...}`, mostrar con `\printbibliography` al final.
- **Apéndices**: dentro de `\begin{appendices}...\end{appendices}`.

---

## 18. Ejemplo mínimo funcional

```latex
\documentclass[12pt, oneside]{book}
\input{config/packages.tex}
\input{config/geometry.tex}
\input{metadata.tex}
\input{themes/catppuccin-palette.tex}
\input{config/hyperref.tex}
\input{config/macros.tex}
\input{config/tcolorbox-config.tex}
\input{environments/generator.tex}
\input{environments/instances.tex}
\input{environments/specials.tex}
\input{config/titles-config.tex}
\input{config/listings-catppuccin.tex}
\input{config/algorithms.tex}
\input{config/tables-config.tex}
\input{config/equation-styles.tex}
\input{config/pgfplots-config.tex}
\input{config/captions-config.tex}

\docTitulo{Mi Libro}
\docAutor{Autor}

\begin{document}
\input{frontmatter/titlepage.tex}
\input{frontmatter/preliminary.tex}
\tableofcontents
\include{chapters/chapter1}
\begin{appendices}
\include{backmatter/appendixA}
\end{appendices}
\end{document}
```

---

**Versión del template**: 0.2  
**Mantenido por**: [J. A. Chala‑Casanova](https://github.com/Zessinthel/)  
**Licencia**: MIT – ver archivo [`LICENSE`](LICENSE)