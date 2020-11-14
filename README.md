# DnD Kata

This Kata aims to model a DnD 5th Edition Charater, with its attributes.
It will introduce somes S.O.L.I.D pratices appyed to OCaml

## Setup a project from scratch

### Create a package

First we need to make an opam package of our project. It's mandatory for Dune that we will use as build system.
We just have to create a file name `<PACKAGE_NAME>.opam`

```sh
touch dnd-kata.opam
```

Since we will not publish our package to [opam repository](http://opam.ocaml.org/packages/) this file may remains empty. If we would, we would have to [add metadata to describe our package](http://opam.ocaml.org/doc/Packaging.html)

### Create a dune project

[Dune](https://dune.build/) is a composable build system for OCaml projects _(and ReasonML and Coq)_. A project is a source tree, maybe containing one or more packages: a typical dune project will have a `dune-project` and one or more `<package>.opam`

So create the `dune-project` and `lang` and `name` :

```sh
echo '(lang dune 2.7.1)\n (name dnd-kata)' >> dune-project
```

You notice that `dune-project` is a manifest that use s-expression format.
It contains the version of Dune we will use and the name of the project.

> You may not be familiar with s-expression. It's just anothe data text format like json, yaml, xml or toml.
> this s-expression
>
> ```
> (lang dune 2.7.1)
> (name dnd-kata)
> ```
>
> can be read as this equivalent json
>
> ```
> {
>  "lang" : {"dune" :  "2.7.1"},
>  "name" : "dnd-kata"
> }
> ```

Each folder and subfolder to include in our package need also a `dune` file manifest. That the case of the root file, so create it:

```sh
echo '(dirs (:standard \ node_modules \ _esy))' >> dune
```

The `dirs` stanza allows specifying the sub-directories dune will include in a build. A directory that is not included by this stanza will not be eagerly scanned by Dune.

`(dirs (:standard \ node_modules \ _esy))` means include all directories except node_modules and \_esy

### Package management with esy

Esy is a toolchain for OCaml _(and ReasonML)_ inspired by the NPM workflow. It use opam and dune with some benefits :

- Unique CLI for the both tools
- Per project dependencies sandboxing
- Use of dependencies coming from many repositories : opam, npm, esy, git, local, ...
- A unique manifest to manage all the dune files

First create the manifest :

```sh
touch package.json
```

and edit it

```json
{
  "name": "dnd-kata",
  "version": "0.0.1",
  "description": "New OCaml project",
  "license": "MPL-2.0",
  "scripts": {
    "start": "esy x dnd"
  },
  "dependencies": {
    "ocaml": "~4.10",
    "@pesy/esy-pesy": "0.1.0-alpha.11",
    "@opam/dune": "2.7.1"
  },
  "devDependencies": {},
  "esy": {
    "build": "dune build -p #{self.name}",
    "buildDev": "pesy build",
    "release": { "releasedBinaries": ["dnd"] }
  }
}
```
