# srw-doc

SRW Documentation

## Requirement

### Using make

Intialize:

```console
make init
```

Run server：

```console
make run
```

Build PDF file

```
make pdf
```

### Using npm

```console
npm install
```

```console
npm run serve
```

### Build

install calibre-book if PDF format is needed (on CentOS)

```
sudo -v && wget -nv -O- https://download.calibre-ebook.com/linux-installer.sh | sudo sh /dev/stdin
```

Build HTML:

```
gitbook build
```

Build PDF:

```
gitbook pdf
```

## Visit

open http://localhost:4000 in browser

