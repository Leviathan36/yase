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