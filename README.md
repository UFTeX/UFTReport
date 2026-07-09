# Manual de Utilização e Elaboração de Relatórios Técnicos e Trabalhos de Disciplina Utilizando a Classe `uftreport.cls`

A classe `uftreport` formata **relatórios técnicos, manuais, projetos e trabalhos de disciplina** do Curso de Ciência da Computação da Universidade Federal do Tocantins (UFT), Campus Universitário de Palmas.

Inspirada no `techrep-ic.sty` do IC/UNICAMP (J. Stolfi et al.) e nas classes `uftmanuais.cls` e `uftreport.cls` da UFT/CC.

---

## 1. Pré-requisitos e Estrutura de Arquivos

```
meu-documento/
├── meu-documento.tex
├── uftreport.cls
└── logos/
    └── logouft.pdf      ← logo da UFT (também aceita .png)
```

## 2. Como Compilar

```bash
pdflatex meu-documento.tex
bibtex   meu-documento        # se houver referências com BibTeX
pdflatex meu-documento.tex
pdflatex meu-documento.tex    # terceira passagem para referências cruzadas
```

> A classe usa o pacote `zref` para determinar a página inicial correta da numeração árabe. São necessárias **ao menos duas passagens** do `pdflatex` para que a paginação fique correta.

## 3. Opções de Classe

```latex
\documentclass[<tipo>, <fonte>]{uftreport}
```

### 3.1. Tamanho de Fonte

* `10pt`
* `11pt`
* `12pt` (Padrão)

### 3.2. Tipo de Documento (obrigatório — escolha um)

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

Para `projectresearch` e `projectextension`, o texto da folha de rosto indica que o documento é um projeto submetido ao curso e lista o orientador (se definido via `\advisor{}`).

```latex
\documentclass[report, 12pt]{uftreport}
\documentclass[projectresearch, 12pt]{uftreport}
\documentclass[projectextension, 12pt]{uftreport}
\documentclass[manual, 12pt]{uftreport}
\documentclass[techreport, 12pt]{uftreport}
```

## 4. Comandos de Metadados

Todos devem ser declarados no **preâmbulo**, antes de `\begin{document}`.

| Comando | Argumentos | Função | Exemplo |
| :--- | :--- | :--- | :--- |
| `\title{#1}` | Título | Título do trabalho. | `\title{Implementação de Somadores Aproximados em FPGA}` |
| `\foreigntitle{#1}` | Título em inglês | Opcional. | `\foreigntitle{Implementation of Approximate Adders on FPGA}` |
| `\author{#1}{#2}` | Nome, Sobrenome | Autor. Repetível para coautores. O primeiro `\author` define `\@authname`/`\@authsurn`, usados na folha de rosto. | `\author{Ana}{Silva}` |
| `\advisor{#1}{#2}{#3}{#4}` | Título, Nome, Sobrenome, Grau | Orientador. Repetível para coorientadores. Se nenhum for declarado, o campo não aparece na capa. | `\advisor{Prof.}{João}{Silva}{Dr.}` |
| `\examiner{#1}{#2}{#3}` | Título, Nome, Sobrenome | Membro da banca examinadora. Repetível. Armazenado, mas não impresso automaticamente — use em folhas de aprovação customizadas. | `\examiner{Prof.}{Marcos}{Lima}` |
| `\department{#1}` | Código | Curso/Unidade Acadêmica (`CC`, `LC`, `EC` ou `EE`; padrão `CC`). | `\department{CC}` |
| `\class{#1}` | Disciplina/Turma | Aparece na capa e na folha de rosto. Omita se o documento não for trabalho de disciplina. | `\class{Algoritmos e Estruturas de Dados II -- 2024.1}` |
| `\TRNumber{#1}` | Número | Usado apenas com `techreport`; gera `UFT-CC-<ano>-<número>` na capa. Padrão `000`. | `\TRNumber{042}` |
| `\date{#1}{#2}{#3}` | Dia, Mês, Ano | Data do documento. | `\date{1}{6}{2025}` |
| `\keyword{#1}` | Palavra-chave | Repetível. Impressa ao final do ambiente `abstract`. | `\keyword{FPGA}` |
| `\foreignkeyword{#1}` | Palavra-chave (EN) | Repetível. Impressa ao final do ambiente `foreignabstract`. | `\foreignkeyword{FPGA}` |
| `\field{#1}` | Campo de pesquisa | Metadado auxiliar, armazenado para uso futuro/folhas customizadas. | `\field{Computação Aproximada}` |

## 5. Estrutura do Documento

```latex
\begin{document}

  % ── Pré-textuais ──────────────────────────────────────────
  \frontmatter          % desativa numeração, estilo vazio
  \maketitle             % imprime a capa
  \makefrontpage         % imprime a folha de rosto (opcional)

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
  \listoftables          % opcional

  % ── Texto principal ───────────────────────────────────────
  \mainmatter            % ativa numeração árabe
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

| Comando | Função |
| :--- | :--- |
| `\frontmatter` | Desativa a numeração de páginas e aplica estilo vazio. Deve vir antes de `\maketitle`. |
| `\maketitle` | Imprime a capa no estilo UFT/CC (moldura azul à esquerda, moldura amarela à direita, caixa central com título e autores, logo e banner institucional no topo). Chamar uma única vez. |
| `\makefrontpage` | Imprime a folha de rosto com autor(es), título e texto descritivo gerado a partir do tipo de documento e da disciplina. |
| `\mainmatter` | Ativa a numeração árabe e restaura o estilo de página com número. Deve vir antes do primeiro `\chapter`. |
| `\ChapterStart{id}{título}` | Marca a página de início do texto principal para que a numeração árabe comece nela. Chamar imediatamente antes do primeiro `\chapter`. `id` é um identificador interno (ex.: `first`); `título` é o título do capítulo correspondente. |
| `\backmatter` | Encerra o texto principal e prepara o ambiente para referências e apêndices. |

## 6. Ambientes Especiais

### `abstract`

```latex
\begin{abstract}
  Texto do resumo.
\end{abstract}
```

As palavras-chave declaradas com `\keyword{}` são impressas automaticamente ao final, precedidas do rótulo **Palavras-chave:** em linha própria.

### `foreignabstract`

```latex
\begin{foreignabstract}
  Abstract text.
\end{foreignabstract}
```

As palavras-chave declaradas com `\foreignkeyword{}` são impressas automaticamente ao final, precedidas de **Keywords:**.

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

Imprime o texto alinhado à direita, no centro vertical da página.

### Citações longas (`quote`)

O ambiente `quote` é redefinido com recuo esquerdo de 4 cm e fonte menor, conforme as normas ABNT:

```latex
\begin{quote}
  Texto da citação longa...
\end{quote}
```

### Listas de símbolos e abreviaturas

```latex
% No preâmbulo:
\makelosymbols
\makeloabbreviations

% No texto:
\symbl{alpha}{Coeficiente de atenuação}
\abbrev{FPGA}{Field-Programmable Gate Array}

% Onde quiser imprimir as listas:
\printlosymbols
\printloabbreviations
```

## 7. Formatação Automática

### Cores institucionais

| Nome | RGB | Uso |
|---|---|---|
| `uftazul` | 0, 74, 128 | Seções, sumário, capa |
| `uftverde` | 0, 133, 119 | Bordas de listagens |
| `uftamarelo` | 253, 185, 19 | Filetes de capítulo, separador da capa |
| `uftcinza` | 100, 100, 100 | Subseções, números de linha |

```latex
{\color{uftazul} texto em azul UFT}
```

### Capítulos e seções

* Cabeçalho de capítulo gerado automaticamente com barra azul e filete amarelo. `manual` usa barra mais alta (1,1 cm) e fonte `\large`; `report`/`techreport` usam barra menor (0,9 cm) e `\normalsize`. Capítulos sem número (`\chapter*{}`) usam o mesmo estilo visual.
* `\section`: negrito azul UFT + linha horizontal. `\subsection`: negrito cinza. `\subsubsection`: cinza, sem negrito. Numeração até `\subsubsection` (`secnumdepth=3`).
* Sumário: todos os níveis exibidos em azul UFT, incluindo números de página.

### Listagens de código

```latex
\begin{lstlisting}[language=Python, caption={Exemplo}]
def hello():
    print("Olá, mundo!")
\end{lstlisting}
```

Configuração automática: numeração de linhas à esquerda; fonte sans-serif `\footnotesize`; fundo cinza claro com borda verde UFT; palavras-chave em azul UFT negrito; comentários em cinza itálico; strings em verde UFT; quebra automática de linhas longas. Para alterar a linguagem padrão, use `\lstset{}` no preâmbulo **após** `\begin{document}`.

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

Contadores independentes de capítulo (numeração contínua). No sumário de figuras/tabelas:

```
Figura 1 -- Descrição
Tabela 3 -- Descrição
```

## 8. Pacotes Carregados Automaticamente

Não é necessário declará-los novamente no preâmbulo:

`fontenc` · `babel` · `inputenc` · `graphicx` · `xcolor` · `geometry` · `setspace` · `indentfirst` · `lastpage` · `amsfonts` · `amsthm` · `amssymb` · `booktabs` · `multirow` · `tabularx` · `wrapfig` · `listings` · `ifthen` · `hyphenat` · `ltxcmds` · `xstring` · `tikz` · `pdfpages` · `wallpaper` · `placeins` · `titlesec` · `chngcntr` · `tocbibind` · `tocloft` · `zref` · `caption` · `hyperref`

Pacotes adicionais específicos do documento devem ser declarados normalmente no preâmbulo.

## 9. Exemplo Completo (Trabalho de Disciplina)

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

Variações rápidas de preâmbulo para os outros tipos de documento:

```latex
% Projeto de pesquisa (PIBIC, TCC)
\documentclass[projectresearch, 12pt]{uftreport}
\title{Exploração de Espaço de Design com GRASP para Filtros Aproximados}
\author{Pedro}{Almeida}
\advisor{Prof.}{João}{Silva}{Dr.}
\department{CC}
\date{1}{6}{2025}
\keyword{Computação Aproximada}
\keyword{Meta-heurísticas}
```

```latex
% Projeto de extensão
\documentclass[projectextension, 12pt]{uftreport}
\title{Oficinas de Programação para Estudantes do Ensino Médio}
\author{Ana}{Costa}
\advisor{Profa.}{Maria}{Souza}{Dra.}
\department{CC}
\date{1}{6}{2025}
\keyword{Extensão Universitária}
\keyword{Ensino de Programação}
```

```latex
% Relatório técnico numerado
\documentclass[techreport, 12pt]{uftreport}
\title{Caracterização de Multiplicadores Aproximados}
\author{João}{Barbosa}
\TRNumber{007}
\department{CC}
\date{15}{3}{2025}
\keyword{Multiplicadores}
\keyword{Computação Aproximada}
```

## 10. Notas e Limitações

- O logo da UFT deve estar em `logos/logouft.pdf` (ou `logos/logouft.png`). Se nenhum arquivo for encontrado, a capa exibe o espaço sem logo.
- Não redefina `\titleformat{\section}`, `\titleformat{\chapter}` ou `hyperref` no preâmbulo — a classe já os configura. Use `\hypersetup{}` para sobrescrever apenas opções específicas (ex.: cor de URL).
- O comando `\and` fica desativado após `\maketitle`. Para múltiplos autores, use chamadas repetidas de `\author{}{}`.
- O nome do arquivo da classe é `uftreport.cls` (sem o sufixo `-cc`).
- Ao usar `abntex2cite`, **não** inclua as linhas `\renewcommand{\backrefpagesname}{}`, `\renewcommand{\backref}{}` nem `\renewcommand*{\backrefalt}[4]{}` no preâmbulo — a classe não carrega o pacote `backref` e esses comandos não existem.