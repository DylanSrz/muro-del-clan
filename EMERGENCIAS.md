# Emergencias

Cada una tiene dos partes: **provocarla** (a propósito, en una rama tuya) y **salir de ella**.
Antes de empezar, siempre:

```bash
git switch develop
git pull
```

---

## 1 · Hice commit en develop en vez de mi rama

**Provócala**

```bash
echo "prueba" > emergencia-<tu-usuario>.txt
git add emergencia-<tu-usuario>.txt
git commit -m "commit en el lugar equivocado"
git push
```

El push va a fallar: develop está protegida. Ahora tienes un commit en tu develop local que no debería estar ahí.

**Sal de ella**

```bash
git switch -c feature/rescate-<tu-usuario>
# la rama nueva se lleva tu commit
git switch develop
git reset --hard origin/develop
# tu develop local vuelve a ser igual al de GitHub
git switch feature/rescate-<tu-usuario>
git log --oneline -3
# tu commit está aquí, a salvo
```

> El orden importa: primero rescatas el commit en la rama nueva, después limpias develop.

---

## 2 · Push rechazado

Necesitan **dos personas** de la célula: A y B.

**Provócala**

A crea la rama y la sube:

```bash
git switch -c practica/celula-N
git push -u origin practica/celula-N
```

B la trae:

```bash
git fetch
git switch practica/celula-N
```

Los dos crean **un archivo distinto** cada uno, con su usuario en el nombre, y hacen commit. A hace push primero. Después B hace push:

```
! [rejected]  practica/celula-N -> practica/celula-N (fetch first)
```

**Sal de ella** (B)

```bash
git pull --rebase
git push
```

> Nunca lo arreglen con `git push --force`: borraría lo que subió A.

---

## 3 · Subí un archivo que no debía

**Provócala**

```bash
git switch -c feature/secreto-<tu-usuario>
echo "API_KEY=12345-no-es-real" > .env
git add .
git commit -m "agrega configuración"
git push -u origin feature/secreto-<tu-usuario>
```

Mira en GitHub: el `.env` está ahí, a la vista de todos.

**Sal de ella**

```bash
echo ".env" >> .gitignore
git rm --cached .env
git add .gitignore
git commit -m "fix: saca .env del repositorio"
git push
```

`--cached` lo saca de Git pero **lo deja en tu carpeta**.

> Importante: el archivo sigue en el historial de commits. Si hubiera sido una clave real, la única solución segura es **cambiar la clave**.

---

## 4 · Me equivoqué en el último commit

**Provócala**

```bash
git switch -c feature/amend-<tu-usuario>
echo "uno" > a-<tu-usuario>.txt
echo "dos" > b-<tu-usuario>.txt
git add a-<tu-usuario>.txt
git commit -m "agrega archvos"
```

Dos errores: el mensaje tiene un error de ortografía y se te olvidó el archivo `b`.

**Sal de ella**

```bash
git add b-<tu-usuario>.txt
git commit --amend -m "feat: agrega archivos a y b"
git log --oneline -2
# un solo commit, corregido
```

> Solo funciona si **todavía no hiciste push**. Si ya lo subiste, haz un commit nuevo con la corrección.

---

## 5 · Tengo cambios a medias y necesito cambiar de rama

**Provócala**

```bash
git switch -c feature/stash-<tu-usuario>
echo "trabajo sin terminar" > pendiente-<tu-usuario>.txt
git add pendiente-<tu-usuario>.txt
git switch develop
```

Git se lleva tus cambios a develop, o se niega a cambiar de rama si chocan. Ninguna de las dos cosas es lo que querías.

**Sal de ella**

```bash
git switch feature/stash-<tu-usuario>
git stash
git status
# carpeta limpia
git switch develop
# ... haces lo que necesitabas ...
git switch feature/stash-<tu-usuario>
git stash pop
# tus cambios volvieron
```

---

## 6 · Mi rama quedó desactualizada

**Provócala**

Crea tu rama, y **no la toques** mientras otro compañero hace merge de algo a develop (por ejemplo, su tarjeta o su palabra del lema).

```bash
git switch -c feature/vieja-<tu-usuario>
```

**Sal de ella**

```bash
git switch develop
git pull
git switch feature/vieja-<tu-usuario>
git merge --no-edit develop
git log --oneline -5
# ya tienes lo último de develop
```

> Háganlo todas las mañanas en su proyecto real. Es la forma más barata de evitar conflictos.

---

## 7 · Necesito deshacer un commit que ya está en GitHub

**Provócala**

```bash
git switch -c feature/revert-<tu-usuario>
echo "esto fue un error" > error-<tu-usuario>.txt
git add error-<tu-usuario>.txt
git commit -m "commit que no debió existir"
git push -u origin feature/revert-<tu-usuario>
```

**Sal de ella**

```bash
git log --oneline -3
# copia el código (hash) del commit que quieres deshacer
git revert --no-edit <hash>
git push
```

`revert` no borra la historia: crea un commit nuevo que hace lo contrario. Por eso es seguro en ramas compartidas.

---

## 8 · Git dice "detached HEAD"

**Provócala**

```bash
git log --oneline -5
# copia el hash de un commit viejo
git checkout <hash>
```

Git te dice que estás en *detached HEAD*: no estás en ninguna rama. Lo que hagas aquí se puede perder.

**Sal de ella**

Si no cambiaste nada:

```bash
git switch develop
```

Si hiciste cambios y los quieres conservar:

```bash
git switch -c rescate/<tu-usuario>
```
