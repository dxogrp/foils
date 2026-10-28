# `foils`

`foils` is a minimal [`FoilTeX`](https://ctan.org/pkg/foiltex)-inspired LaTeX slide class. It is built on
the standard `article` class and provides a small slide-deck API with
landscape letter pages, large sans-serif typography, section and foil title
pages, appendix numbering, and a bibliography helper.

## Installation

For local use, place `foils/foils.cls` where LaTeX can find it. The simplest
project layout is:

```text
my-talk/
  talk.tex
  foils.cls
```

Then use the class from `talk.tex`:

```latex
\documentclass{foils}
```

For repeated use, install `foils.cls` into a directory searched by your TeX
distribution, such as a local `texmf` tree.

## Quick Start

```latex
\documentclass{foils}

\title{A Short Talk}
\author{Firstname Lastname}
\date{\today}
\info{Example Slides}

\begin{document}
\maketitle

\makesection{First section}
\begin{itemize}
  \item First topic
  \item Second topic
\end{itemize}

\makefoil{A first foil}
\begin{itemize}
  \item The Foils class uses large slide-friendly type.
  \item Each foil starts on a new landscape page.
  \item Footers show the current section and page number.
\end{itemize}

\makefoil{A second foil}
\[
  E = mc^2
\]

\makeappendix

\makefoil{Backup material}
Appendix foils are numbered A1, A2, and so on.
\end{document}
```

Compile with your usual LaTeX engine, for example:

```sh
pdflatex talk.tex
```

## Class Options

`169`
: Use 16:9 slides with a page width of 16 inches and a height of 9 inches:
  `\documentclass[169]{foils}`. Margins, typography, and other settings stay
  the same. Without this option, pages remain 11 by 8.5 inches.

`lefttitles`
: Make titles created by `\makefoil` flush left. By default, these titles are
  centered in both page formats. For widescreen slides with flush-left titles,
  use `\documentclass[169,lefttitles]{foils}`.

`lecture`
: Reset page numbering at each section and print foil numbers as
  `section--page`, for example `2--3`.

All options can be combined, for example
`\documentclass[169,lefttitles,lecture]{foils}`.

Footers and `\pageref` use matching page numbers:

| Situation | Normal mode | `lecture` mode |
| --- | --- | --- |
| Page 1 before any section | `1` | `0–1` |
| Section 4, page 11 | `11` | `4–11` |
| Start of section 5 | Continues consecutive numbering | `5–1` |
| First appendix foil | `A1` | `A1` |
| Next appendix foil | `A2` | `A2` |
| New section within appendix | Continues appendix numbering | Continues appendix numbering |

## Public Commands

`\info{<text>}`
: Set small metadata text printed on section title pages.

`\setfoilcolor{<color>}`
: Set one shared color for section headings, the Appendix heading, foil titles,
  and footer labels and page numbers. The default is black. The class loads
  `xcolor`, so named colors, color mixes, and custom colors are supported.
  Set the color in the preamble, for example:

  ```latex
  \definecolor{accent}{HTML}{204A87}
  \setfoilcolor{accent}
  ```

  Alternatively, use a mix such as `\setfoilcolor{blue!70!black}`.

`\maketitle`
: Make the deck title page from `\title`, `\author`, and `\date`.
  The article-style `\thanks` command is not supported.

`\makesection{<title>}`
: Start a new numbered section. In `lecture` mode, the page counter is reset
  to `1` for the section. This command advances LaTeX's standard `section`
  counter, so manual adjustments can use `\setcounter{section}{<number>}`.

`\makefoil{<title>}`
: Start a new foil with a centered title, or a flush-left title with the `lefttitles`
  option. The footer uses the current section title and the current page number.

`\makeappendix`
: Start the appendix and switch subsequent footer numbers and `\pageref`
  references to `A1`, `A2`, and so on, in both normal and `lecture` modes.

`\makereference[<size>]{<bib-file>}`
: Start an unnumbered bibliography page using the `alpha` bibliography style.
  The optional size defaults to `\small`. Direct `\bibliography` calls are not
  supported.

`\vf`
: Insert stretchable vertical space with `\vspace*{\fill}`.

Inherited article commands that do not match the foil model, including
`\section`, `\subsection`, `\subsubsection`, `\paragraph`, `\subparagraph`,
`\part`, `\appendix`, `\tableofcontents`, `\listoffigures`, and
`\listoftables`, report class errors. The article-style `abstract`,
`titlepage`, and `theindex` environments are disabled too. Use `\makesection`,
`\makefoil`, and `\makeappendix` instead.

## Manual

A practical LaTeX manual is available at `doc/foils.tex`. Compile it with:

```sh
cd doc && pdflatex foils.tex
```

## Example

A sample deck adapted from the classic FoilTeX sample topics is available at
`sample/foils-sample.tex`. Compile it from
the repository root with:

```sh
TEXINPUTS=foils: pdflatex -output-directory sample sample/foils-sample.tex
```

## License

Foils is licensed under the Apache License 2.0. See [LICENSE](./LICENSE).
