# blog

## Install

```
CGO_ENABLED=1 go install -tags extended github.com/gohugoio/hugo@latest
```

## Theme

- [Generate a theme](https://panr.github.io/terminal-css/)
- Use the `Terminal theme scheme` radio button
- Download and plop the files in the top level static dir

## Develop

```
go server --bind 0.0.0.0 --port 21313 --baseURL "http://x.x.x.x:21313"
```

## Content

- [Images](https://unsplash.com/)

## Template overrides

- content/_index provides unsafe HTML for the about me box
- layouts/index:
    - Add a "Recent posts" h1
    - Change the date/author meta to be in-line with the h2 post title to squish
    - Remove post tags
