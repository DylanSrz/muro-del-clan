# Muro del clan

Taller de Git. Cada misión termina con algo tuyo apareciendo en el muro:

**https://dylansrz.github.io/muro-del-clan/**

> En Windows usen **Git Bash** para todo el taller, no PowerShell. Así los comandos son iguales para todos.
>
> Donde veas `<tu-usuario>`, pon tu usuario de GitHub **en minúsculas**. Donde veas `N`, el número de tu célula.

---

## Misión 0 · Antes del jueves (obligatoria)

Si llegas al taller sin esto, te vas a quedar atrás los primeros 20 minutos.

**1. Acepta la invitación al repositorio**

https://github.com/DylanSrz/muro-del-clan/invitations

**2. Dile a Git quién eres** (el correo debe ser el de tu cuenta de GitHub)

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tucorreo@ejemplo.com"
```

**3. Cambia el editor de Git a VS Code** (si no, algunos comandos te abren Vim y no vas a saber salir)

```bash
git config --global core.editor "code --wait"
```

**4. Clona y prueba que puedes subir algo**

```bash
git clone https://github.com/DylanSrz/muro-del-clan.git
cd muro-del-clan
git status
git switch -c prueba/<tu-usuario>
git push -u origin prueba/<tu-usuario>
```

- Si se abre el navegador pidiendo que inicies sesión en GitHub: acepta. Es normal, pasa solo la primera vez.
- Si dice `403` o `Permission denied`: no aceptaste la invitación del paso 1.
- Si dice `Everything up-to-date` o crea la rama: **listo, estás preparado.**

---

## Misión 1 · Tu tarjeta en el muro

Cada uno trabaja en **su propio archivo**, así que aquí no puede haber conflictos.

```bash
git switch develop
git pull
git switch -c feature/tarjeta-<tu-usuario>
cp PLANTILLA-TARJETA.md _tarjetas/<tu-usuario>.md
```

Abre `_tarjetas/<tu-usuario>.md` y llénalo. **No borres las comillas ni las líneas `---`.**

```bash
git status
git add _tarjetas/<tu-usuario>.md
git commit -m "feat: agrega tarjeta de <tu-nombre>"
git push -u origin feature/tarjeta-<tu-usuario>
```

## Misión 2 · Pull request con revisión

1. Entra al repositorio en GitHub. Va a aparecer un botón amarillo **Compare & pull request**.
2. Verifica que diga **base: develop** ← **compare: feature/tarjeta-<tu-usuario>**.
3. Pon un título y crea el PR.
4. Pídele a **alguien de tu célula** que lo revise: pestaña *Files changed* → *Review changes* → **Approve**.
5. Con la aprobación, **tú** le das *Merge pull request*.
6. Actualiza tu develop local:

```bash
git switch develop
git pull
```

En uno o dos minutos tu tarjeta aparece en el muro.

**Si te toca revisar:** mira que el número de célula esté bien y que el archivo no tenga errores. Si algo está mal, usa *Request changes* en vez de aprobar.

---

## Misión 3 · El lema de la célula, palabra por palabra

Cada célula tiene un archivo con su lema: `_includes/lema-celula-N.html`. Ahora mismo solo dice `...`

**La regla:** cada integrante agrega **una sola palabra**. Todos editan la misma línea del mismo archivo. El conflicto está garantizado, y esa es la idea.

**Primera parte: todos al mismo tiempo**

```bash
git switch develop
git pull
git switch -c feature/lema-<tu-usuario>
```

Abre `_includes/lema-celula-N.html`, borra los `...` y escribe **una palabra**. Nada más.

```bash
git add _includes/lema-celula-N.html
git commit -m "feat: palabra de <tu-nombre> para el lema"
git push -u origin feature/lema-<tu-usuario>
```

Abre tu PR hacia develop.

**Segunda parte: por turnos**

El copiloto de la célula aprueba y hace merge del **primer** PR. A partir de ahí, los demás PR van a decir *This branch has conflicts that must be resolved*.

Cuando el copiloto diga que es tu turno:

```bash
git switch develop
git pull
git switch feature/lema-<tu-usuario>
git merge --no-edit develop
```

Git te va a avisar del conflicto. Abre el archivo y verás algo así:

```
<<<<<<< HEAD
rápido
=======
Código limpio
>>>>>>> develop
```

**Resuélvelo:** deja todas las palabras en **una sola línea**, en el orden que acuerde la célula, y **borra las tres líneas de marcadores**. Debe quedar solo:

```
Código limpio rápido
```

```bash
git add _includes/lema-celula-N.html
git commit -m "merge: resuelve conflicto del lema"
git push
```

Tu PR ya no tiene conflictos. Alguien lo aprueba, haces merge, y le toca al siguiente.

> Si subes el archivo con los marcadores todavía adentro, el muro lo va a mostrar en rojo frente a todo el clan. 😅

**¿Te perdiste a mitad del merge?** `git merge --abort` deja todo como estaba antes.

---

## Misión 4 · Emergencias

Cada célula recibe dos emergencias de [EMERGENCIAS.md](EMERGENCIAS.md). Primero **la provocan a propósito**, después **la resuelven**. Al final, cada célula le explica una al resto del clan.

| Célula | Emergencias |
|---|---|
| 1 | 1 y 5 |
| 2 | 2 y 6 |
| 3 | 3 y 7 |
| 4 | 4 y 8 |
| 5 | 2 y 7 |

---

## Si algo sale mal

1. `git status` — casi siempre te dice qué está pasando.
2. `git merge --abort` — si estás a mitad de un merge y te perdiste.
3. **¿Quedaste atrapado en una pantalla negra con `~` a la izquierda?** Es Vim. Escribe `:wq` y presiona Enter.
4. **No borres la carpeta para clonar de nuevo.** Levanta la mano o llama a tu copiloto.
