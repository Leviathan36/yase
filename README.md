# Local deploy with Jekyll

## Install Jekyll

[install jekyll](https://jekyllrb.com/docs/installation/)

## Install dependencies
```bash
cd <project_dir>
bundle install
```

## Serve the site
```bash
bundle exec jekyll serve
```

## Index layout
When there are many articles, add an additional level to the index. Instead of placing the various articles in the main index, place the article categories (cybersecurity, devops, software engineering, etc.) and each category will point to a sub-index containing all the articles for that subcategory.

## Directories explanation
- _includes: custom html file to override the ones of the theme
- _site: this folder is created local istance of Jekyll and it's included into .gitignore
- assets: custom css (for this specific site not for the jekyll theme)
- imgs: cover images for individual articles
- <other_dirs>: individual articles