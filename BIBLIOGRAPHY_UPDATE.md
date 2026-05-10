# Bibliography Workflow

The journal ultimately asks for a single `.tex` file with the reference list
embedded in the manuscript. That does not mean the manuscript has to be edited
that way during drafting.

The recommended workflow is:

- draft with a normal BibTeX bibliography;
- keep `sn-bibliography.bib` as the source of truth;
- paste the generated `.bbl` reference list into `sn-article.tex` only when
  preparing the final submission file.

## During Drafting

Use the normal BibTeX commands in `sn-article.tex`:

```tex
\bibliographystyle{bst/sn-nature}
\bibliography{sn-bibliography}
```

Then add, remove, edit, or reorder references in the usual way:

1. Edit `sn-bibliography.bib`.
2. Edit citations in `sn-article.tex`.
3. Compile with BibTeX:

   ```sh
   pdflatex sn-article.tex
   bibtex sn-article
   pdflatex sn-article.tex
   pdflatex sn-article.tex
   ```

This is the easiest mode while writing because the bibliography updates
automatically from the `.bib` file.

## Final Single-File Submission

Only when the manuscript is ready for submission:

1. Run the normal BibTeX compile cycle above.
2. Open `sn-article.bbl`.
3. Copy the complete block from:

   ```tex
   \begin{thebibliography}{...}
   ```

   through:

   ```tex
   \end{thebibliography}
   ```

4. Paste that block into `sn-article.tex` where the bibliography commands were.
5. Remove or comment out:

   ```tex
   \bibliographystyle{bst/sn-nature}
   \bibliography{sn-bibliography}
   ```

6. Compile the single `.tex` file twice with `pdflatex` and check for warnings.

If the generated `.bbl` contains `\bibcommenthead`, add this compatibility line
before it, or put it in the preamble:

```tex
\providecommand{\bibcommenthead}{}
```

This is needed because the manuscript uses plain `article.cls` rather than
`sn-jnl.cls`.

## Capitalization In BibTeX

The Nature `.bst` lowercases titles. Protect acronyms, proper names, and
special capitalization with braces in `sn-bibliography.bib`:

```bibtex
title = {{EuroSCORE} {II}}
title = {Delirium ... ({CAM-ICU})}
title = {{ECG}-derived sympathetic ...}
title = {{RetinaFace}: Single-Shot Multi-Level Face Localisation in the Wild}
title = {{U-Net}: Convolutional networks for biomedical image segmentation}
```

For surnames with particles, use the natural BibTeX form:

```bibtex
author = {van den Oord, Aaron and Li, Yazhe and Vinyals, Oriol}
```

## Troubleshooting

- `I didn't find a database entry`: a citation key in `sn-article.tex` is not in
  `sn-bibliography.bib`. Add the missing BibTeX record or fix the citation key.
- `Repeated entry`: the same BibTeX key appears more than once. Remove or rename
  one copy.
- `You can't pop an empty literal stack`: this is a known `sn-nature.bst` quirk
  for some `@inproceedings` entries in this project. If `sn-article.bbl` is
  still produced and the final `pdflatex` build is clean, the reference list can
  still be used.

The important distinction: `sn-bibliography.bib` is the working bibliography;
the embedded `thebibliography` block is only a final submission artifact.