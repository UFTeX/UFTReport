# uftreport — Classe LaTeX UFT/CC

Classe LaTeX para produção de relatórios técnicos, manuais, projetos e trabalhos de
disciplinas do **Curso de Ciência da Computação** da Universidade Federal do
Tocantins (UFT), Campus Universitário de Palmas.

Inspirada no `techrep-ic.sty` do IC/UNICAMP (J. Stolfi et al.) e nas classes
`uftmanuais.cls` e `uftreport.cls` da UFT/CC.

---

## Estrutura de arquivos esperada

```
meu-documento/
├── meu-documento.tex
├── uftreport.cls
└── logos/
    └── logouft.pdf      ← logo da UFT (também aceita .png)
```

---

## Opções de classe

```latex
\documentclass[<tipo>, <fonte>]{uftreport}
```

### Tipo de documento (obrigatório — escolha um)

| Opção | Descrição |
|---|---|
| `report` | Trabalho de disciplina ou relatório acadêmico **(padrão)** |
| `projectresearch` | Projeto de pesquisa — PIBIC, TCC, proposta de pesquisa |
| `projectextension` | Projeto de extensão |
| `manual` | Manual técnico — cabeçalhos de capítulo maiores |
| `techreport` | Relatório técnico numerado — exibe número TR na capa |

O tipo influencia:
- A altura e o estilo do cabeçalho de capítulo (`\chapter`)
- O texto da linha de tipo/data na caixa de conteúdo da capa
- O texto gerado automaticamente na folha de rosto (`\makefrontpage`)

Para `projectresearch` e `projectextension`, o texto da folha de rosto indica
que o documento é um projeto submetido ao curso e lista o orientador (se
definido via `\advisor{}`).

### Tamanho de fonte (opcional)

| Opção | Descrição |
|---|---|
| `10pt` | Fonte 10pt |
| `11pt` | Fonte 11pt |
| `12pt` | Fonte 12pt **(padrão)** |

### Exemplo completo

```latex
\documentclass[report, 12pt]{uftreport}
\documentclass[projectresearch, 12pt]{uftreport}
\documentclass[projectextension, 12pt]{uftreport}
\documentclass[manual, 12pt]{uftreport}
\documentclass[techreport, 12pt]{uftreport}
```

---

## Comandos de metadados

Todos devem ser declarados no **preâmbulo**, antes de `\begin{document}`.

### Título

```latex
\title{Título do Trabalho}
\foreigntitle{Title of the Work}   % título em inglês (opcional)
```

### Autor(es)

```latex
\author{Nome}{Sobrenome}
```

Pode ser chamado múltiplas vezes para adicionar coautores:

```latex
\author{Ana}{Silva}
\author{Bruno}{Costa}
```

O primeiro `\author` define `\@authname` e `\@authsurn`, usados internamente
na folha de rosto.

### Orientador(es)

```latex
\advisor{Título}{Nome}{Sobrenome}{Grau}
```

| Parâmetro | Exemplo |
|---|---|
| Título | `Prof.` / `Profa.` |
| Nome | `João` |
| Sobrenome | `Silva` |
| Grau | `Dr.` / `Dra.` / `Me.` |

Pode ser chamado múltiplas vezes para adicionar coorientadores. Se nenhum
orientador for declarado, o campo não aparece na capa.

```latex
\advisor{Prof.}{João}{Silva}{Dr.}
\advisor{Profa.}{Maria}{Souza}{Dra.}
```

### Banca examinadora

```latex
\examiner{Título}{Nome}{Sobrenome}
```

Pode ser chamado múltiplas vezes. Armazenado internamente mas não impresso
automaticamente — use conforme necessário em folhas de aprovação customizadas.

### Curso / Departamento

```latex
\department{código}
```

| Código | Nome completo |
|---|---|
| `CC` | Ciência da Computação **(padrão)** |
| `LC` | Licenciatura em Computação |
| `EC` | Engenharia Civil |
| `EE` | Engenharia Elétrica |

### Disciplina / Turma

```latex
\class{Algoritmos e Estruturas de Dados II -- 2024.1}
```

Quando definido, aparece na linha de tipo/data da capa e no texto da folha de
rosto. Omita o comando se o documento não for um trabalho de disciplina.

### Número do relatório técnico

Usado apenas com a opção `techreport`:

```latex
\TRNumber{042}
```

Gera a identificação `UFT-CC-<ano>-042` na capa. O valor padrão é `000`.

### Data

```latex
\date{dia}{mês}{ano}
```

```latex
\date{1}{6}{2025}
```

### Palavras-chave

```latex
\keyword{Computação Aproximada}
\keyword{FPGA}
\keyword{Meta-heurísticas}
```

Cada chamada adiciona uma palavra-chave. Aparecem automaticamente ao final do
ambiente `abstract`.

### Palavras-chave em inglês

```latex
\foreignkeyword{Approximate Computing}
\foreignkeyword{FPGA}
\foreignkeyword{Metaheuristics}
```

Aparecem automaticamente ao final do ambiente `foreignabstract`.

### Campos de pesquisa (metadado auxiliar)

```latex
\field{Computação Aproximada}
\field{Síntese de Circuitos}
```

Armazenado internamente para uso futuro ou em folhas customizadas.

---

## Estrutura do documento

O documento deve seguir esta sequência:

```latex
\begin{document}

  % ── Pré-textuais ──────────────────────────────────────────
  \frontmatter          % desativa numeração, estilo vazio
  \maketitle            % imprime a capa
  \makefrontpage        % imprime a folha de rosto (opcional)

  % Elementos opcionais, nesta ordem:
  \dedication{Texto da dedicatória.}

  \begin{acknowledgement}
    Texto dos agradecimentos.
  \end{acknowledgement}

  \begin{abstract}
    Texto do resumo em português.
  \end{abstract}

  \begin{foreignabstract}
    Abstract text in English.
  \end{foreignabstract}

  \tableofcontents
  \listoffigures        % opcional
  \listoftables         % opcional

  % ── Texto principal ───────────────────────────────────────
  \mainmatter           % ativa numeração árabe
  \ChapterStart{first}{Introdução}   % marca a página inicial

  \chapter{Introdução}
  ...

  % ── Pós-textuais ──────────────────────────────────────────
  \backmatter

  \begin{thebibliography}{9}
    ...
  \end{thebibliography}

\end{document}
```

### `\frontmatter`

Desativa a numeração de páginas e aplica estilo vazio. Deve vir antes de
`\maketitle`.

### `\maketitle`

Imprime a capa no estilo UFT/CC (inspirado no IC/UNICAMP): moldura azul à
esquerda, moldura amarela à direita, caixa central única com título e autores,
logo e banner institucional no topo. Deve ser chamado uma única vez.

### `\makefrontpage`

Imprime a folha de rosto com nome do(s) autor(es), título e texto descritivo
gerado automaticamente a partir do tipo de documento e da disciplina definida.

### `\mainmatter`

Ativa a numeração árabe e restaura o estilo de página com número. Deve vir
antes do primeiro `\chapter`.

### `\ChapterStart{id}{título}`

Marca a página de início do texto principal para que a numeração árabe comece
nela, não na primeira página após `\mainmatter`. Deve ser chamado imediatamente
antes do primeiro `\chapter`.

| Parâmetro | Descrição |
|---|---|
| `id` | Identificador interno (ex.: `first`) |
| `título` | Título do capítulo correspondente |

```latex
\mainmatter
\ChapterStart{first}{Introdução}
\chapter{Introdução}
```

### `\backmatter`

Encerra o texto principal e prepara o ambiente para referências e apêndices.

---

## Ambientes pré-textuais

### `abstract`

```latex
\begin{abstract}
  Texto do resumo.
\end{abstract}
```

As palavras-chave declaradas com `\keyword{}` são impressas automaticamente ao
final, separadas da última linha por espaço e precedidas pelo rótulo
**Palavras-chave:** em uma linha própria.

### `foreignabstract`

```latex
\begin{foreignabstract}
  Abstract text.
\end{foreignabstract}
```

As palavras-chave declaradas com `\foreignkeyword{}` são impressas
automaticamente ao final, precedidas por **Keywords:** em uma linha própria.

### `acknowledgement`

```latex
\begin{acknowledgement}
  Texto dos agradecimentos.
\end{acknowledgement}
```

### `\dedication{texto}`

```latex
\dedication{À minha família, pelo apoio incondicional.}
```

Imprime o texto alinhado à direita no centro vertical da página.

---

## Formatação automática

### Cores institucionais

Definidas como cores nomeadas e disponíveis em todo o documento:

| Nome | RGB | Uso |
|---|---|---|
| `uftazul` | 0, 74, 128 | Seções, sumário, capa |
| `uftverde` | 0, 133, 119 | Bordas de listagens |
| `uftamarelo` | 253, 185, 19 | Filetes de capítulo, separador da capa |
| `uftcinza` | 100, 100, 100 | Subseções, números de linha |

Exemplo de uso em texto:

```latex
{\color{uftazul} texto em azul UFT}
```

### Capítulos

O cabeçalho de capítulo é gerado automaticamente com barra azul e filete
amarelo. O estilo varia conforme o tipo de documento:

- **`manual`**: barra mais alta (1,1 cm), fonte `\large`
- **`report` / `techreport`**: barra menor (0,9 cm), fonte `\normalsize`

Capítulos sem número (`\chapter*{}`) usam o mesmo estilo visual.

### Seções

| Nível | Formatação |
|---|---|
| `\section` | Negrito azul UFT + linha horizontal |
| `\subsection` | Negrito cinza |
| `\subsubsection` | Cinza, sem negrito |

A numeração vai até o nível `\subsubsection` (`secnumdepth=3`).

### Sumário

Todos os níveis (capítulo, seção, subseção, subsubseção) são exibidos em
**azul UFT**, incluindo os números de página.

### Listagens de código

O ambiente `lstlisting` vem pré-configurado:

```latex
\begin{lstlisting}[language=Python, caption={Exemplo}]
def hello():
    print("Olá, mundo!")
\end{lstlisting}
```

Configurações aplicadas automaticamente:

- Numeração de linhas à esquerda
- Fonte sans-serif em tamanho `\footnotesize`
- Fundo cinza claro, borda verde UFT
- Palavras-chave em azul UFT em negrito
- Comentários em cinza itálico
- Strings em verde UFT
- Quebra automática de linhas longas

Para alterar a linguagem padrão ou outras opções, use `\lstset{}` no preâmbulo
**após** `\begin{document}` — as configurações da classe servem como base.

### Margens e espaçamento

| Configuração | Valor |
|---|---|
| Papel | A4 |
| Margem superior | 3 cm |
| Margem inferior | 2 cm |
| Margem esquerda | 3 cm |
| Margem direita | 2 cm |
| Espaçamento entre linhas | 1,5 |
| Recuo de parágrafo | 1,25 cm |

### Figuras e tabelas

Os contadores de figura e tabela são independentes de capítulo (numeração
contínua ao longo do documento).

No sumário de figuras e tabelas, os rótulos aparecem como:

```
Figura 1 -- Descrição
Tabela 3 -- Descrição
```

### Citações longas

O ambiente `quote` é redefinido com recuo esquerdo de 4 cm e fonte menor,
conforme as normas ABNT:

```latex
\begin{quote}
  Texto da citação longa...
\end{quote}
```

---

## Listas auxiliares

### Lista de símbolos

No preâmbulo:

```latex
\makelosymbols
```

No texto, para registrar um símbolo:

```latex
\symbl{alpha}{Coeficiente de atenuação}
```

Para imprimir a lista:

```latex
\printlosymbols
```

### Lista de abreviaturas

No preâmbulo:

```latex
\makeloabbreviations
```

No texto:

```latex
\abbrev{FPGA}{Field-Programmable Gate Array}
```

Para imprimir:

```latex
\printloabbreviations
```

---

## Pacotes carregados automaticamente

A classe carrega os seguintes pacotes. **Não é necessário declará-los
novamente no preâmbulo:**

`fontenc` · `babel` · `inputenc` · `graphicx` · `xcolor` · `geometry` ·
`setspace` · `indentfirst` · `lastpage` · `amsfonts` · `amsthm` · `amssymb` ·
`booktabs` · `multirow` · `tabularx` · `wrapfig` · `listings` · `ifthen` ·
`hyphenat` · `ltxcmds` · `xstring` · `tikz` · `pdfpages` · `wallpaper` ·
`placeins` · `titlesec` · `chngcntr` · `tocbibind` · `tocloft` · `zref` ·
`caption` · `hyperref`

Pacotes adicionais específicos do documento devem ser declarados normalmente
no preâmbulo.

---

## Compilação

```bash
pdflatex meu-documento.tex
bibtex   meu-documento        # se houver referências com BibTeX
pdflatex meu-documento.tex
pdflatex meu-documento.tex    # terceira passagem para referências cruzadas
```

> A classe usa o pacote `zref` para determinar a página inicial correta da
> numeração árabe. São necessárias **ao menos duas passagens** do `pdflatex`
> para que a paginação fique correta.

---

## Exemplo mínimo

```latex
\documentclass[report, 12pt]{uftreport}

\title{Implementação de Somadores Aproximados em FPGA}
\foreigntitle{Implementation of Approximate Adders on FPGA}

\author{Ana}{Souza}
\advisor{Prof.}{Carlos}{Lima}{Dr.}

\department{CC}
\class{Projeto de Graduação I -- 2025.1}
\date{1}{6}{2025}

\keyword{Computação Aproximada}
\keyword{FPGA}
\keyword{Verilog}

\foreignkeyword{Approximate Computing}
\foreignkeyword{FPGA}
\foreignkeyword{Verilog}

\begin{document}

\frontmatter
\maketitle
\makefrontpage

\begin{abstract}
  Este trabalho investiga somadores aproximados em FPGA.
\end{abstract}

\begin{foreignabstract}
  This work investigates approximate adders on FPGA.
\end{foreignabstract}

\tableofcontents

\mainmatter
\ChapterStart{first}{Introdução}

\chapter{Introdução}

Texto do capítulo.

\backmatter

\begin{thebibliography}{9}
  \bibitem{ref1} AUTOR, A. \textit{Título}. Editora, 2024.
\end{thebibliography}

\end{document}
```

---

## Exemplo para projeto de pesquisa (PIBIC, TCC)

```latex
\documentclass[projectresearch, 12pt]{uftreport}

\title{Exploração de Espaço de Design com GRASP para Filtros Aproximados}
\author{Pedro}{Almeida}
\advisor{Prof.}{João}{Silva}{Dr.}

\department{CC}
\date{1}{6}{2025}

\keyword{Computação Aproximada}
\keyword{Meta-heurísticas}
```

---

## Exemplo para projeto de extensão

```latex
\documentclass[projectextension, 12pt]{uftreport}

\title{Oficinas de Programação para Estudantes do Ensino Médio}
\author{Ana}{Costa}
\advisor{Profa.}{Maria}{Souza}{Dra.}

\department{CC}
\date{1}{6}{2025}

\keyword{Extensão Universitária}
\keyword{Ensino de Programação}
```

---

## Exemplo para relatório técnico numerado

```latex
\documentclass[techreport, 12pt]{uftreport}

\title{Caracterização de Multiplicadores Aproximados}
\author{João}{Barbosa}
\TRNumber{007}
\department{CC}
\date{15}{3}{2025}

\keyword{Multiplicadores}
\keyword{Computação Aproximada}
```

---

## Notas

- O logo da UFT deve estar em `logos/logouft.pdf` (ou `logos/logouft.png`).
  Se nenhum arquivo for encontrado, a capa exibe o espaço sem logo.
- Não redefina `\titleformat{\section}`, `\titleformat{\chapter}` ou
  `hyperref` no preâmbulo — a classe já os configura. Use `\hypersetup{}` para
  sobrescrever apenas opções específicas (ex.: cor de URL).
- O comando `\and` fica desativado após `\maketitle`. Para múltiplos autores,
  use chamadas repetidas de `\author{}{}`.
- O nome do arquivo da classe é `uftreport.cls` (sem o sufixo `-cc`).
- Ao usar `abntex2cite`, **não** inclua as linhas
  `\renewcommand{\backrefpagesname}{}`, `\renewcommand{\backref}{}` nem
  `\renewcommand*{\backrefalt}[4]{}` no preâmbulo — a classe não carrega o
  pacote `backref` e esses comandos não existem.