# LaTeX Flipbook

Delight readers of your LaTeX document or thesis with a [flip book](https://en.wikipedia.org/wiki/Flip_book) effect when they rapidly flick through the printed pages or the PDF.

## Usage

**Make sure to check the [`example.tex`](example.tex) file for a sample implementation of how to use this package.**

- [Download](https://github.com/nilsleiffischer/latex-flipbook/archive/master.zip) the `flipbook.sty` file, drop it into the same directory as your `.tex` document and load the `flipbook` package in your document preamble:

    ```latex
    \usepackage{flipbook}
    ```
- Save the frames for your flipbook animation as files named `{prefix}{framenumber}.{extension}`. You can use any file format you like:

  - Ideally, save the frames as `.pgf` files so that they are rendered natively in the document. No additional code on your part is necessary.
  - Alternatively, save `.tex` files that each load an image or PDF with `\includegraphics`, draw a `tikz` figure or anything else that LaTeX can do. These files you would usually generate with a script, such as in Python.
  - If you have a list of files that LaTeX cannot `\input`, such as images, or if you wish to specify a different filename format, then `\renewcommand` the `\inputflipbookframe` macro in whichever way you need. For instance, for images this code could be

    ```latex
    \renewcommand{\inputflipbookframe}[3]{\includegraphics{#1#2.#3}}
    ```

  where `#1` is the common prefix, `#2` is the frame number and `#3` is the filename extension.
- At the same position on every page, for instance in the header or footer as in `example.tex`, place calls to `\flipbookframe` or `\labeledflipbookframe` to render the animation frame for the page you call it on.

  - Use `\flipbookframe` to only render the animation frame, e.g. like so:

    ```latex
    \flipbookframe{flipbook_frames/frame_}[pgf]
    ```
  - Use `\labeledflipbookframe` to render an additional label, such as the page number, next to the frame, e.g. like so:

    ```latex
    \labeledflipbookframe{flipbook_frames/frame_}[pgf]{c}{\thepage}{2em}%
    ```

## Available macros

- `\inputflipbookframe`: Renew this macro to load arbitrary file formats or file name conventions.

  _Arguments:_

  - `#1`: Path to the frame file including common filename prefix, but excluding frame number
  - `#2`: Frame number
  - `#3`: Filename extension

- `\flipbookframe`: Render the animation frame for the current page.

  _Arguments:_

  - `#1` (optional): The frame number offset from the first page number. A positive integer makes the first document page correspond to a later frame.
  - `#2` (optional): Frames per page. A number larger than 1 results in a speedup of the flipbook, where frames are skipped. A number smaller that one slows down the animation by displaying the same frame on multiple pages. Set this argument to 0.5 to keep the standard animation speed when printing a two-sided document.
  - `#3`: The path to the flipbook frame files, including the common filename prefix. The default implementation of \inputflipbookframe appends an underscore and the frame number, followed by a dot and argument `#4`.
  - `#4` (optional): The filename ending. Default is `tex`.
  - `#5` (optional): Use to scale the frames. Default is `1`.

- `\labeledflipbookframe`: Render the frame for the current page, and a corresponding label (e.g. the page number).

  _Arguments:_

  - `#1` through `#5`: Same as for `\flipbookframe`.
  - `#6`: Label alignment. `l` and `r` align the label left and right of the flipbook frame, respectively. `c` centers the label, and renders the flipbook on its left.
  - `#7`: The label text, e.g. `\thepage`.
  - `#8`: Width of the label box, thereby determining its distance to the flipbook animation.
