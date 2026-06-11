# Cómo colaborar con el repo de Pedro (sin entorpecerle)

> Guía para quien aporta desde un *fork* (p. ej. `colegatron`). Pensada para usar
> Git sin ser experto. La regla que lo resume todo está al final.

---

## El modelo mental: 3 sitios

```
   Repo de Pedro  ──(tú LEES)──►  Tu fork (origin)  ◄──(tú ESCRIBES)──  Tu ordenador
   "upstream"                     "colegatron"                          (clon local)
   no escribes                    tu copia pública
```

- **`upstream`** = el repo de Pedro. Tú **lees** de aquí; no escribes.
- **`origin`** = tu fork en GitHub. Aquí **sí** escribes (haces *push*).
- **Tu ordenador** = donde editas.

**Regla de oro:** solo haces *push* a **tu fork**. Nunca al repo de Pedro. Por eso
es imposible entorpecerle: tus cambios viven en tu repo hasta que **él** los toma.

---

## Enviarle cambios a Pedro → un *Pull Request* (PR)

Un PR es una **propuesta**, no una subida a su repo. Tu código sigue en tu fork; el
PR solo le dice a Pedro: *"mira esto y tómalo si quieres"*. Nada entra en su repo
hasta que **él pulsa Merge**.

Pasos (en la web de GitHub, logueado con tu cuenta del fork):
1. Entra en **tu fork**.
2. **"Contribute" → "Open pull request"** (o pestaña *Pull requests → New*).
3. Elige **base** = repo de Pedro / rama destino, y **compare** = tu rama.
4. Título + descripción claros → **Create pull request**.

> **Un PR por idea.** Así Pedro acepta unos y rechaza otros, en vez de "todo o nada".

### Mejor aún: PR contra una rama suya, no contra `main`

Si Pedro crea una rama de recepción (p. ej. `colaboraciones`), abre el PR con
**base = esa rama**. Así, al integrarlo, su `main` **ni se toca**: él prueba y elige
qué pasar a `main` cuando quiera. (La rama destino debe existir ya en su repo; eso
lo crea él.)

---

## Traerte lo nuevo de Pedro → "Sync fork"

Cuando Pedro actualice su `main`, ponte al día **antes** de empezar algo nuevo:

- **Web:** en tu fork, botón **"Sync fork" → "Update branch"**.
- **Terminal:**
  ```bash
  git fetch upstream
  git switch main
  git merge upstream/main
  ```

---

## El ciclo completo, paso a paso (terminal)

```bash
# 1. Partir de lo último de Pedro
git fetch upstream && git switch main && git merge upstream/main

# 2. Crear una rama para tu cambio
git switch -c mi-cambio

# 3. ...editas los ficheros...

# 4. Guardar y subir A TU FORK
git add -A
git commit -m "Descripción del cambio"
git push origin mi-cambio

# 5. Abrir el PR (en la web, o con gh):
#    gh pr create --repo <repo-de-pedro> --base colaboraciones --head <tu-fork>:mi-cambio
```

---

## Resumen de un vistazo

| Quiero… | En la web | En terminal |
|---|---|---|
| Traerme lo de Pedro | **Sync fork** | `git fetch upstream && git merge upstream/main` |
| Empezar un cambio | — | `git switch -c mi-cambio` |
| Subirlo **a mi fork** | — | `git push origin mi-cambio` |
| Proponérselo a Pedro | **Contribute → Open pull request** | `gh pr create …` |

> **En una frase:** editas en una rama → *push* **a tu fork** → abres un **PR** →
> Pedro integra lo que quiera. Tú nunca tocas su repo.

---

Ver también: [para-pedro-colaboracion-git.md](para-pedro-colaboracion-git.md) — cómo
Pedro, desde su lado, escoge **solo los ficheros que quiere** de las colaboraciones.
