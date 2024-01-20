# How to add Jupyter Book to another Jupyter Book

## Adding to a new or existing book
We're going to add the repository of the external book to the repository of the parent book by using Git Submodules. This will basically nest the external repository in the parent repository, and it will appear as if we've manually copied the entire repository into the parent repository. I will use this repository as an example parent repository, and I'm going to add parts of the MUDE book from last year to it. You can find the old book [here](https://github.com/tudelft-citg/mude).

First, go to the location where the external book should live. I will put it in `book/external` to highlight the fact that the content is in fact part of an external book.

  mkdir -p book/external
  cd book/external

Next, we'll add the MUDE book as a submodule:

  git submodule add https://github.com/tudelft-citg/mude.git

You can see that the `book/external` directory now contains a directory called `MUDE`, so the result is equivalent to simply running a `git clone` inside `book/external`. What is important to note here is that the contents of `book/external/MUDE` are not part of the parent repository. Instead, `book/external/MUDE` is a fully functional Git Repository itself. This means that you can make changes to the external book, from inside the parent book. After the `git submodule` command, you should make a commit:

  git commit -m "Add external book"

Now, you can add sections of the external book to the table of contents:

  chapters:
  - file: external/MUDE/book/intro.md

## Cloning
If you're cloning a repository that features submodules, the directories of the submodules will not be populated by default. To fix that, you need to do a recursive clone (i.e., clone the parent repository, as well as the submodules):

  git clone --recurse-submodules <link to parent repository>

## The external book is updated
When you add the external book as a submodule to your repository, Git will pin its version. When the external book is updated, you'll need to manually pull the updates to the parent book. To this end, you can use the following command:

  git submodule update --remote

## More info
[Here](https://git-scm.com/book/en/v2/Git-Tools-Submodules) you can find more information on Git submodules.

