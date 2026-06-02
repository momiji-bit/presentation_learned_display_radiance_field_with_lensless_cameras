# Welcome to [Slidev](https://github.com/slidevjs/slidev)!

To start the slide show:

- `npm install`
- `npm run dev`
- visit http://localhost:3030

Edit the [slides.md](./slides.md) to see the changes.

Learn more about Slidev on [documentations](https://sli.dev/).

## Export to PDF
To export the slides to a PDF file. Make sure that you have `Playwright` installed first:

```shell
npm i -D playwright-chromium
```

Once you have done so, use the following syntax to export your slides from your root folder:


```shell
npx slidev export --executable-path /snap/bin/chromium --dark --format pdf --with-clicks --output slides
```

Note that you can change the format from `pdf` to `png` or `pptx` to output other formats.
`--executable-path` helps you specify the path to `chromium`, in my case, I identified it using `which chromium` syntax in my shell.
Note that `--executable-path` helps you make sure to render videos correctly in your export.
`--with-clicks` flag helps you to preserve animation by slide advancing.
`--dark` helps you choose the dark mode in the rendering of your slides.
Finally, `--output` flag helps you specify the name of the `pdf` file that you want to export to.

If you are interested in compressing the size of the PDF generated through above routine, please consider using the following syntax:

```shell
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.5 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -dPrinted=false -sOutputFile=slides_low_res.pdf slides.pdf
```

Note that you can switch to different compression sizes using `-dPDFSETTINGS` flag. For instance, you can use `\screen` for 72 dpi image resolution, or `\ebook` for 150 dpi.

# Deploying as a website
Make sure to have `.github/workflows/deploy.yml` from this repository. In your repository, go to `Settings` > `Pages`. Under `Build` and `deployment`, select GitHub Actions. Finally, after the workflows are executed, a link to the slides should appear under `Settings` > `Pages`. Make sure to have your default branch as `main` but not `master` as `master` is a protected name causes problems in the deployment. If your some of your layout images are not visible online, consider placing them under `public` folder.
