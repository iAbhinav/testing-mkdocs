# MkDocs

---

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Getting Started

### Python

Instal python 3 if not already

* MacOS
```bash
brew install python3
pip3 install --upgrade pip
```

### Installation


```
pip3 install mkdocs
```

> The following are optional if you  want to use the material theme for the static website

```bash
pip3 install mkdocs-material
pip3 install mkdocs-material-extensions
```
> More on [Material for Mkdocs](https://squidfunk.github.io/mkdocs-material/getting-started/).
### Project

* Create

```
python3 -m mkdocs new folder-name
```

* Serve

```
python3 -m mkdocs serve
```

* Build

```bash
python3 -m mkdocs build
```

### Theme

To add a theme to the project add this to the `mkdocs.yml` file.

```yml
theme: "material"
```

### Logo
To change logo of the project add this to the `mkdocs.yml` file.

```yaml
theme:
name: 'material'
logo: 'resources/images/logo/umun/800x269.png'
favicon: 'resources/images/logo/umun/800x269.png'
```

here `resources` is a folder in this hierarchy

```text
-docs
    -resources
        -images
            -logo
               ...
-mkdocs.yml
```

