# Create a book based on several other books using git submodule

The goal is to build a jupyter book based on the template and several
other files in existing books (submodules). We should be able to choose a
specific version of the files.

Assumptions:

- no change/commit to the files in submodules. also no need to update the
  submodules to a new version.
- submodules have been generated according to the [TeachBooks
  template](https://github.com/TeachBooks/template).

In general, the command [`git submodule
add`](https://git-scm.com/docs/gitsubmodules) clones a repository inside another
repository. For that you need the following:

- to have access to the submodule repository,
- to have enough disk space to clone the submodule repository,
- to check the license compatibility of the repositories.

## User input variables

In `_toc.yml` you can define the external file you want to include in your book.
The syntax below is suggested to have unique paths for each file:

```yaml
`external/<organization_name>/<repository_name>/<tag_branch_commit>/<path_to_file>`.
```

## Workflow

1. Create the `url` e.g.
   `https://github.com/<organization_name>/<repository_name>.git` based on
   information in the `_toc.yml` for adding submodule. In case the repository is
   on gitlab, the first part of the `url` would be `https://gitlab.com/`.
2. Perform checks:
    - Check if the `url` exists and have access to it.
    - Check if the submodule has a `LICENSE` file. If not, check the license of
      the repository. If no information is available, stop the workflow.
    - Check if the submodule contains the files you want to include in the book,
      see `_toc.yml`.
    - Check if the submodule contains another submodule; by inspecting
      `.gitmodules` files in submodules. If yes, you need run `git submodule
      update --init --recursive`.
    - Check if the submodule contains a `requirements.txt` file. If yes, make
      sure to install the requirements.
    - Check if submodule has the `tag_branch_commit`. If not, use the latest
      release or the main branch.
    - Check `_config.yml` in each submodule and merge it with your book
      `_config.yml`, if possible.
    - ... (add more checks if needed)
3. Create the `path` e.g. `book/external/<organization_name>/<repository_name>/<tag_branch_commit>`.
4. Add the submodule to the book using the command `git submodule add <url>
   <path>`. This will clone the other repository in the `book` directory of the
    template. To save space and also avoid any broken components, you may
    include `-b <branch>` in the submodule command. However, this does not work
    for a commit or tag, see next step.
5. Change directory to `path` and checkout the specific `tag_branch_commit` in
   the submodule `git checkout tag_branch_commit`.
6. (optional) to save space and also avoid any broken components, you may remove
   the files that are not needed in the submodule. For example, you may remove
   everything except the specified files. Howevere, this may cause problems if
   the file contains relative links. You may also set configs in the
  `_config.yml`, see example below.
7. Build the book using `jupyter-book build book/` in template repo.
8. If everything is fine, commit `.gitmodules`, and `_toc.yml` and paths (staged
   by git submodule) and push to your book template repository.

## Example

This is an example without any checks explained in the workflow.

1. add new files to `_toc.yml`:

  ```yaml
    - file: external/tudelft-citg/mude/main/book/reliability-component/overview.md
    - file: external/TeachBooks/Showing-Physics/main/book/Introduction/Python summary
    - file: external/TUDelft-CITG/OpenCLSim/v1.6.2/book/Command_basics.md
  ```

2. Add the submodules, this will create subdirectories in the `book/external/`:

  ```bash
  git submodule add https://github.com/tudelft-citg/mude book/external/tudelft-citg/mude/main
  git submodule add https://github.com/TeachBooks/Showing-Physics book/external/TeachBooks/Showing-Physics/main
  git submodule add https://github.com/TUDelft-CITG/OpenCLSim book/external/TUDelft-CITG/OpenCLSim/v1.6.2
  ```

3. Checkout if not main branch:

    ```bash
    cd book/external/TUDelft-CITG/OpenCLSim/v1.6.2
    git checkout v1.6.2
    ```

4. Install requirements if needed:
    You may check every `requirements.txt` file in the submodules and install them:

    ```bash
    pip install -r requirements.txt
    ```

5. Build the book:

To avoid warnings e.g. `WARNING: document isn't included in any toctree`, you
may use the following settings in the `_config.yml`:

  ```yaml
  only_build_toc_files: true
  exclude_patterns: [.github, .gitignore, .git]
  ```

Then build the book:

    ```bash
    jupyter-book build book/
    ```
