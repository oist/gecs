# Good Enough Computing in Science (GECS)<br><small>Mini Course at OIST

GECS (pron. "geeks") introduces software development into scientific research project workflow. The course is taught at OIST as a Mini Course.

## Website
[https://oist.github.io/gecs](https://oist.github.io/gecs)
Built with [Quarto](https://quarto.org/docs/websites/) and hosted with [GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site).

### Deployment

The course website is authored on the `main` branch. On every push to `main`,
the [Quarto publish workflow](.github/workflows/publish.yml) renders the site
and publishes the generated files to the `gh-pages` branch. GitHub Pages serves
that branch from its root directory.

To publish an update, edit the Quarto source files, commit the changes, and
push them to `main`. The workflow performs the Quarto render and deployment;
generated `_site/` files do not need to be committed manually.

## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

## License

[MIT](https://choosealicense.com/licenses/mit/)
