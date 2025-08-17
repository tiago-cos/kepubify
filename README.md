<h1 align="center">kepubify</h1>

**Kepubify converts EPUBs to Kobo EPUBs.** <a href="https://github.com/pgaskin/kepubify/actions/workflows/kepubify.yml"><img src="https://github.com/pgaskin/kepubify/actions/workflows/kepubify.yml/badge.svg?branch=master&event=push" align="right"/></a>

**[`Website`](https://pgaskin.net/kepubify/)** &nbsp; **[`Download`](https://pgaskin.net/kepubify/dl/)** &nbsp; **[`Documentation`](https://pgaskin.net/kepubify/docs/)** &nbsp; **[`Web Version`](https://pgaskin.net/kepubify/try/)** &nbsp; **[`pkg.go.dev`](https://pkg.go.dev/github.com/pgaskin/kepubify/v4)**

> **Note:** This fork modifies kepubify to add Kobo spans to SVG elements and the generated title page. Its purpose is to avoid issues with progress synchronization via Kobo spans.  
> Since this isn’t a general bug fix, I don’t intend to merge it upstream.

## About

Kepubify is standalone (it also works as a library or a webapp), converts most books in a fraction of a second (40-80x faster than Calibre), handles malformed HTML/XHTML without causing further issues, has multiple optional conversion options (punctuation smartening, custom CSS, text replacement, and more), has a full test suite, is interoperable with other applications, and is safe to use with untrusted books.

Two additional standalone utilities are included with kepubify. [`covergen`](./cmd/covergen) pre-generates cover images to speed up library browsing on Kobo eReaders while providing higher-quality resizing. [`seriesmeta`](./cmd/seriesmeta) scans for EPUBs and KEPUBs, and updates the Kobo database with the Calibre or EPUB3 series metadata.

See the [releases](https://github.com/pgaskin/kepubify/releases/latest) page for pre-built binaries for Windows, Linux, and macOS. See the [website](https://pgaskin.net/kepubify/) for more [documentation](https://pgaskin.net/kepubify/docs/), pre-built [binaries](https://pgaskin.net/kepubify/dl/) for Windows, Linux, and macOS, and a [web version](https://pgaskin.net/kepubify/try/).
 
## Building

Kepubify requires Go 1.16 or later. To install kepubify directly, run `go install github.com/pgaskin/kepubify@latest`. To build from source, clone this repository, and run `go build ./cmd/kepubify`.

On Go 1.17 or later, additional optimizations are automatically used to significantly improve kepubify's performance by preventing unchanged files from being re-compressed. To use a [backported](https://github.com/pgaskin/kepubify/tree/forks/go116-zip.go117) version of these optimizations on Go 1.16, add the option `-tags zip117` to the build/install command. If you are using kepubify as a library in another application with `-tags zip117` enabled on Go 1.16, it must also use the backported package when passing a `*zip.Reader` to `(*kepub.Converter).Transform`.

To build `seriesmeta`, a C compiler must be installed and CGO must be enabled.

Note that kepubify uses a custom [fork](https://github.com/pgaskin/kepubify/tree/forks/html) of [`golang.org/x/net/html`](https://pkg.go.dev/golang.org/x/net/html). This fork provides additional options used by kepubify to allow reading malformed HTML/XHTML and to produce polyglot HTML/XHTML output for maximum compatibility. Previously, kepubify replaced it using a `replace` directive in `go.mod`, but since the fork is now a standalone package, this is not necessary anymore, and will no longer cause conflicts if used as a dependency in applications requiring `golang.org/x/net/html` directly.

## Branches

This repository contains multiple main branches with code used by kepubify.

| Branch | Description | Status |
| --- | --- | --- |
| [master](https://github.com/pgaskin/kepubify/tree/master) | Kepubify, seriesmeta, and covergen (this branch). | [![kepubify](https://github.com/pgaskin/kepubify/actions/workflows/kepubify.yml/badge.svg?branch=master&event=push)](https://github.com/pgaskin/kepubify/actions/workflows/kepubify.yml) |
| [gh-pages](https://github.com/pgaskin/kepubify/tree/gh-pages) | Kepubify website and JS/WASM library. | [![website](https://github.com/pgaskin/kepubify/actions/workflows/website.yml/badge.svg?branch=gh-pages&event=push)](https://github.com/pgaskin/kepubify/actions/workflows/website.yml) |
| [forks/html](https://github.com/pgaskin/kepubify/tree/forks/html) | Fork of [golang.org/x/net/html](https://pkg.go.dev/golang.org/x/net/html) with additional features and modifications required by kepubify. | [![forks - html](https://github.com/pgaskin/kepubify/actions/workflows/_html.yml/badge.svg?branch=forks%2Fhtml&event=push)](https://github.com/pgaskin/kepubify/actions/workflows/_html.yml) |
| [forks/go116-zip.go117](https://github.com/pgaskin/kepubify/tree/forks/go116-zip.go117) | Fork of [archive/zip](https://pkg.go.dev/archive/zip) from Go 1.17 backported to Go 1.16 for performance improvements. | [![forks - go116-zip.go117](https://github.com/pgaskin/kepubify/actions/workflows/_go116-zip.go117.yml/badge.svg?branch=forks%2Fgo116-zip.go117&event=push)](https://github.com/pgaskin/kepubify/actions/workflows/_go116-zip.go117.yml) |
