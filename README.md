
## Usage

```yml
      - name: Create commit comment
        uses: restreamio/github-action-commit-comment@latest
        with:
          body: |
            This is a multi-line test comment
            - With GitHub **Markdown** :sparkles:
            - Created by [commit-comment][1]

            [1]: https://github.com/restreamio/github-action-commit-comment
```

## Action inputs

| Name | Description | Default |
| --- | --- | --- |
| `token` | `GITHUB_TOKEN` or a `repo` scoped [PAT](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token). | `GITHUB_TOKEN` |
| `repository` | The full name of the target repository. | `github.repository` (current repository) |
| `sha` | The commit SHA. | `github.sha` OR, for `pull_request` events `github.event.pull_request.head.sha` |
| `body` | (**required**) The contents of the comment. | |
| `path` | Relative path of the file to comment on. | |
| `position` | Line index in the diff to comment on. | |

## Example

Here is an example setting optional input parameters.

```yml
      - name: Create commit comment
        uses: restreamio/github-action-commit-comment@latest
        with:
          sha: 843dea1cc2e721163c20a5c876b5b155f7f3aa75
          body: |
            This is a multi-line test comment
            - With GitHub **Markdown** :sparkles:
            - Created by [commit-comment][1]

            [1]: https://github.com/restreamio/github-action-commit-comment
          path: path/to/file.txt
          position: 1
```

### Setting the comment body from a file

This example shows how file content can be read into a variable and passed to the action.
The content must be [escaped to preserve newlines](https://github.community/t/set-output-truncates-multiline-strings/16852/3).

```yml
      - id: get-comment-body
        run: |
          body=$(cat comment-body.txt)
          body="${body//'%'/'%25'}"
          body="${body//$'\n'/'%0A'}"
          body="${body//$'\r'/'%0D'}" 
          echo ::set-output name=body::$body

      - name: Create commit comment
        uses: restreamio/github-action-commit-comment@latest
        with:
          body: ${{ steps.get-comment-body.outputs.body }}
```

### Accessing commits in other repositories

You can create a commit comment in another repository by using a [PAT](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) instead of `GITHUB_TOKEN`.
The user associated with the PAT must have write access to the repository.

## License

[MIT](LICENSE)
