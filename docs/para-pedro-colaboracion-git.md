# Guía para Pedro — recibir colaboraciones y elegir qué integrar

> Pensada para usar Git **sin ser experto**. Copia y pega los comandos tal cual.
> La idea de fondo: las aportaciones de otros **nunca tocan tu `main`** hasta que
> tú lo decides, y puedes quedarte **solo con lo que quieras** (incluso fichero a
> fichero).

---

## 1. Cómo te llega una colaboración

Quien colabora (p. ej. Iván) **no escribe en tu repositorio**. Trabaja en su
propia copia (su *fork*) y te manda una **propuesta** (un *Pull Request*).

Para que esas propuestas no toquen tu `main`, usamos una **rama de recepción**
en tu repo, por ejemplo `colaboraciones`. Los PR se integran ahí, no en `main`.

### Crear esa rama (una sola vez)

En la web de GitHub, en tu repositorio:
1. Pulsa el selector de ramas (donde pone `main`).
2. Escribe `colaboraciones` y pulsa **"Create branch: colaboraciones"**.

O en el terminal:
```bash
git switch main
git switch -c colaboraciones
git push origin colaboraciones
```

A partir de ahí, los PR se abren **contra `colaboraciones`**. Cuando aceptas uno,
entra en esa rama y **tu `main` sigue intacto**.

---

## 2. Coger SOLO algunos ficheros para tu `main`

Esta es la parte clave: en `colaboraciones` hay varias cosas, pero tú solo
quieres unos ficheros concretos en `main`.

**Paso 1 — Ponte en `main`:**
```bash
git switch main
```

**Paso 2 — Mira qué ficheros son distintos (para decidir):**
```bash
git diff --stat main colaboraciones
```

**Paso 3 — (Opcional) Mira el contenido de uno concreto antes de decidir:**
```bash
git diff main colaboraciones -- MPS.py
```

**Paso 4 — Trae SOLO los ficheros que quieras** (escribe sus rutas):
```bash
git checkout colaboraciones -- MPS.py assets/pedro.jpg
```
Eso copia esos ficheros a tu `main` tal como están en `colaboraciones`.
El resto de la rama **ni se toca**.

**Paso 5 — Revisa y confirma:**
```bash
git status                                   # comprueba qué va a entrar
git commit -m "Integro MPS.py y la foto desde colaboraciones"
```

**Paso 6 — (Si quieres publicarlo) súbelo a tu repo:**
```bash
git push origin main
```

---

## 3. ¿Y si de un fichero solo quiero algunos trozos?

`git checkout colaboraciones -- fichero` se trae el **fichero entero**. Si de un
mismo fichero solo quieres una parte, usa el modo interactivo:

```bash
git checkout -p colaboraciones -- MPS.py
```

Git te irá enseñando los cambios **por trozos** y te preguntará en cada uno:
*¿aplicar este cambio? (y/n)*. Tú eliges trozo a trozo.

---

## 4. Red de seguridad (recomendado)

Si te da respeto tocar `main`, hazlo primero en una rama de prueba; si sale mal,
la borras y `main` ni se entera:

```bash
git switch -c prueba main                 # rama temporal a partir de main
git checkout colaboraciones -- MPS.py     # trae lo que quieras
git commit -m "prueba"
# ...míralo, ábrelo, comprueba que está bien...

git switch main
git merge prueba                          # ahora sí, lo pasas a main
git branch -D prueba                      # borra la rama temporal
```

Si algo no te gustó en la prueba: `git switch main` y `git branch -D prueba`.
No habrá pasado nada en `main`.

---

## 5. Resumen de un vistazo

| Quiero… | Comando |
|---|---|
| Ver qué ficheros difieren | `git diff --stat main colaboraciones` |
| Traer ficheros concretos a `main` | `git checkout colaboraciones -- ruta/fichero …` |
| Traer solo algunos trozos de un fichero | `git checkout -p colaboraciones -- ruta/fichero` |
| Guardar lo integrado | `git commit -m "..."` |
| Publicarlo | `git push origin main` |

> En una frase: **`git checkout colaboraciones -- <los ficheros que quieras>`**
> y luego **`git commit`**. Eso es "me quedo solo con esto".
