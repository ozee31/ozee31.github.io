# Website with Jekyll

## Installation

### Clone repository
```shell
git clone git@github.com:ozee31/ozee31.github.io.git website
cd website
git checkout -b dev origin/dev

git clone git@github.com:ozee31/ozee31.github.io.git _site
```

### Install jekyll

```shell
gem install jekyll
gem install jekyll-paginate
gem install jekyll-feed
```

## Run dev server in VM

```shell
jekyll serve --host 0.0.0.0 --watch --force_polling
```

In brower run [http://localhost:4000/](http://localhost:4000/)

## Build

```shell
JEKYLL_ENV=production jekyll build
```