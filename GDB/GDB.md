# GDB Cheat Sheet

## Lancement et contrôle
| Commande          | Raccourci | Description                                |
| ----------------- | --------- | ------------------------------------------ |
| `gdb ./programme` | -         | Lance GDB avec le programme                |
| `run`             | `r`       | Exécute le programme                       |
| `Ctrl+C`          | -         | Arrête l'exécution sans quitter GDB        |
| `continue`        | `c`       | Reprend l'exécution après un arrêt         |
| `quit`            | `q`       | Quitte GDB                                 |
| `kill`            | -         | Tue le programme en cours sans quitter GDB |

## Navigation dans le code
| Commande             | Raccourci | Description                                      |
|----------------------|-----------|-------------------------------------------------|
| `next`               | `n`       | Avance à la ligne suivante (sans entrer)       |
| `step`               | `s`       | Avance et entre dans les fonctions            |
| `finish`             | -         | Termine la fonction actuelle et revient        |
| `until <ligne>`      | `u`       | Avance jusqu’à une ligne spécifique            |
| `start`              | -         | Retourne au début de `main` et s’arrête        |

## Breakpoints
| Commande                | Raccourci | Description                                   |
| ----------------------- | --------- | --------------------------------------------- |
| `break <fonction>`      | `b`       | Place un breakpoint sur une fonction          |
| `break <fichier:ligne>` | `b`       | Place un breakpoint à une ligne               |
| `break ... if <cond>`   | -         | Breakpoint conditionnel (ex: `if x > 10`)     |
| `info breakpoints`      | `info b`  | Liste les breakpoints                         |
| `disable <num>`         | -         | Désactive un breakpoint (numéro via `info b`) |
| `enable <num>`          | -         | Réactive un breakpoint                        |
| `delete <num>`          | `d`       | Supprime un breakpoint (ou tous si vide)      |

## Inspection
| Commande             | Raccourci | Description                                      |
|----------------------|-----------|-------------------------------------------------|
| `backtrace`          | `bt`      | Affiche la pile d’appels                       |
| `frame <num>`        | `f`       | Change de frame dans la pile                   |
| `info locals`        | -         | Liste les variables locales                    |
| `print <variable>`   | `p`       | Affiche la valeur d’une variable               |
| `print $registre`    | `p`       | Affiche un registre (ex: `$rsi` pour x)        |
| `x/<n>f <adresse>`   | -         | Affiche la mémoire (ex: `x/10f $rdi`)          |
| `info registers`     | `i r`     | Liste les valeurs des registres                |

## Automatisation et Signaux
| Commande             | Raccourci | Description                                      |
|----------------------|-----------|-------------------------------------------------|
| `commands <num>`     | -         | Ajoute des commandes à exécuter au breakpoint  |
| `catch signal SIGSEGV` | -       | Capture un signal comme SIGSEGV                |
| `disassemble`        | `disas`   | Affiche l’assembleur de la fonction actuelle   |

## Exemple d’utilisation
```gdb
gdb ./snake-project
(gdb) break FloodFill.cpp:85
(gdb) run
[Arrêt]
(gdb) print nx
(gdb) next
(gdb) Ctrl+C  # Arrête sans quitter
(gdb) backtrace
(gdb) continue
(gdb) quit
```
