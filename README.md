# Share content between books

```{admonition} User types
:class: tip
This section is useful for user type 4-6.
```

## Adding to a new or existing book
If you want to add the repository of an external book to the repository of your parent book you can do so by using Git Submodules. This will basically nest the external repository in the parent repository, and it will appear as if we've manually copied the entire repository into the parent repository. I will use [this repository](https://github.com/TeachBooks/Nested-Books) as an example parent repository, and I'm going to add parts of an old [MUDE book](https://github.com/tudelft-citg/mude).

First, let's define the location where the external book should live. Good practise is to put it in `book/external` to highlight the fact that the content is in fact part of an external book. So the first step is to create the subdirectory `book/external/` in your repository. If you'd like to do that with the CLI you can do so as follows:

    mkdir -p book/external

Now we can add the external book in this folder. You'll need to use the CLI for that (open the CLI by opening Git Bash or click `Repository` - `Open in command prompt` in GitHub Desktop):

    cd book/external
    git submodule add <SSH-clone link to external book (https://github.com/tudelft-citg/mude.git for example)>

You can see that the `book/external` directory now contains a directory with the name of the external repository (`MUDE` or example), so the result is equivalent to simply running a `git clone` inside `book/external`. What is important to note here is that the contents of `book/external/<external repository>` are not part of the parent repository. Instead, `book/external/`book/external/<external repository>` is a fully functional Git Repository itself. This means that you can make changes to the external book, from inside the parent book. After the `git submodule` command, you should make a commit:

````{tab-set}
```{tab-item} ... using CLI
    git commit -m "Add external book"
```
```{tab-item} ... using GitHub Desktop
    ...
```
````

Now, you can add sections of the external book to `_toc.yml`:

    chapters:
    - file: external/MUDE/book/intro.md

## Cloning
If you're cloning a repository that features submodules, the directories of the submodules will not be populated by default. To fix that, you need to do a recursive clone (i.e., clone the parent repository, as well as the submodules):

    git clone --recurse-submodules <link to parent repository>

## The external book is updated
When you add the external book as a submodule to your repository, Git will pin its version. When the external book is updated, you'll need to manually pull the updates to the parent book. GitHub Desktop will notify you on this. To this end, you can use the following command:

````{tab-set}
```{tab-item} ... using CLI
    git submodule update --remote
```
```{tab-item} ... using GitHub Desktop
    ...
```
````

## Build book on GitLab/GitHub with submodule
Doesn't work yet, see https://github.com/TeachBooks/Nested-Books/issues/1

## More info
[Here](https://git-scm.com/book/en/v2/Git-Tools-Submodules) you can find more information on Git submodules.

