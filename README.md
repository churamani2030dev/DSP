
name: Build PDF from Markdown
on: [push, workflow_dispatch]

jobs:
  md_to_pdf:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install pandoc + TeX
        run: |
          sudo apt-get update
          sudo apt-get install -y pandoc texlive-xetex texlive-fonts-recommended

      - name: Convert README.md → README.pdf
        run: |
          pandoc README.md -o README.pdf --pdf-engine=xelatex -V geometry:margin=1in

      # Option 1: download from Actions artifacts
      - uses: actions/upload-artifact@v4
        with:
          name: pdfs
          path: README.pdf

      # Option 2 (optional): commit PDF back into the repo
      # - name: Commit PDF back
      #   uses: stefanzweifel/git-auto-commit-action@v5
      #   with:
      #     commit_message: "chore: build README.pdf"
      #     file_pattern: README.pdf
