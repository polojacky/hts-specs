## Generating PDF specification documents

Use the _Makefile_ to generate PDFs from the TeX source documents.
Both TeX source and generated PDFs are checked into the **master** branch, so the make rules are set up to stage PDFs into a _new/_ subdirectory, from where they can be copied when you are ready to check them in.

Most of the specifications use a _.ver_ file and associated rules to display a commit hash and datestamp on their title page.
(See _SAMv1.tex_ and _new/SAMv1.pdf_'s _Makefile_ dependencies for how to add this to other specifications.)
So the usual workflow when editing these documents is (for example, when working on the SAM specification):

1. Edit _SAMv1.tex_, and type `make new/SAMv1.pdf` to generate a working PDF to preview your work.

2. When you are ready, commit your _.tex_ source changes (but don't commit any changed PDF files yet).

3. Type `make clean SAMv1.pdf` to regenerate the PDF and copy it to the main directory.
(Optionally, verify that it contains the correct commit hash for your source changes.)
Now commit your _.pdf_ changes, separately from any source changes.

### Rationale

It is a little inconvenient having the working PDFs down in a subdirectory, but this is outweighed by the convenience of being able to switch between Git branches etc without trouble — as there would be if updated working PDFs were in the main directory, overwriting the checked-in PDFs.

The intention is that the commit hash embedded in a PDF encompasses all the source changes and commits that contribute to that PDF.
The hash of the particular commit that updates the PDF is of course not yet known when the PDF is being generated, so the best that can be done is the hash of a slightly-previous commit.
Therefore:
* The PDF needs to be committed separately from the corresponding TeX source changes.
* The PDF should not be updated in a merge commit (as commits from one or the other of the merge's parents will not be recorded), and there's not much point updating it in a pull request.
* So pull requests need to be merged, and then their PDFs updated separately as a non-merge commit on **master**.
* If a series of changes are being made or several pull requests are being merged at once, the PDF updates can be batched up and just made once at the end.
* Conversely, if there are changes pending to several (even unrelated) PDFs, there is no reason not to commit them all at once.

If you are working on several PDFs at once, be careful in step 3 and perhaps use `make clean new/VCFv4.2.pdf new/VCFv4.3.pdf; make VCFv4.2.pdf VCFv4.3.pdf` to ensure that spurious “-dirty” commit hashes don't make their way into your PDFs.

<!-- vim:set linebreak: -->
