---
title: Remplazando la historia con git
tags:
  - git
  - programming
---

Bueno, me presentaron un desafío de lo más interesante, queremos cortar la
historia en git, tenemos una historia muy larga y queremos cortarla en dos
partes:

```
A -> .... -> B -> C -> D -> ... -> E

# y queremos algo como:

A -> .... -> B (old-master)
C (new-root) -> ... -> E (new-master)
```

De pronto `git-rebase` no alcanza, porque bien podríamos hacer:

```bash
git checkout -b old-master B
git checkout --orphan="new-root" C
git checkout -b new-master E
git rebase --onto new-root --root C # o algo así
```

Pero ahí se complica (al menos en este caso), porque la historia entre C y E
puede ser complicada para un rebase, con merges conflictivos que habría que
volver a resolver y ese camino no funciona así de fácil.

Así que hay que usar una herramienta más poderosa (y más peligrosa) y muuuy
lenta, que es [git-filter-branch][git-filter-branch], o si tenés ganas de
instalar, la recomendada en la documentación: [git-filter-repo][].

El problema es que usar ésta herramienta así es bastante complicado, pero por
suerte hay otro comando que va a hacer la mitad del trabajo (la mitad
complicada): [git-replace][], que lo que hace es instruir a git a reemplazar las
referencias a un commit por otro:

``` bash
git replace --graft D new-root
```

Y esto que hace?, bueno indica que queremos modificar D, para en vez de ver al D
original ver a un D' que es igualito, excepto por sus parents (que en git es el
graft). Así que después de correr ese comando el git log va a quedar algo así
como:

```
C (new-root) -> D' (replaced) -> ... -> E (new-master)
```

Y ya estaríamos casi listos, excepto por un detalle y es que éste cambio está
solo ocurriendo en nuestro repositorio local, porque el hay un archivo que le
indica a git que haga eso... ahora necesitamos nuevos commits, pero eso no es
tan difícil, ya que ahora git-filter-branch se va a encargar de todo sin ningún
parametro, porque va a reescribir la historia haciendo nuestro cambio
permanente.

``` bash
git filter-branch
```

Después de eso, le podemos decir a git que abandone su esquizofrenia, borrando
el archivo que fue creado en `.git/refs/replace/` por `git-replace`.

Si a alguien se le ocurre otro uso de estos comandos tan raros, no duden en
contactarme.

[git-filter-branch]: https://git-scm.com/docs/git-filter-branch
[git-replace]: https://git-scm.com/docs/git-replace
[git-filter-repo]: https://github.com/newren/git-filter-repo

