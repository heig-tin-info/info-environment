# Compilateur C/C++

Compiler un programme en C ou en C++ requiert un compilateur. Il s'agit d'un **programme** qui permet de traduire le code source en langage C ou C++ en un **fichier objet** (extension `.o` ou `.obj`), ou un **fichier exécutable** (extension `.exe` sous Windows ou sans extension dans un système POSIX).

Le compilateur le plus utilisé pour le C est `gcc` (GNU Compiler Collection) et pour le C++ est `g++`. Ces deux compilateurs sont inclus dans la suite logicielle `gcc`. Pour installer `gcc` sous Ubuntu, il suffit de taper la commande suivante dans un terminal :

```bash
sudo apt-get install build-essential
```

Sous Windows, c'est plus compliqué. GCC étant un logiciel prévu pour un système POSIX, il n'est pas directement compatible avec Windows. Il existe cependant des ports de GCC pour Windows, comme [MinGW](http://www.mingw.org/), [Cygwin](https://www.cygwin.com/) ou [MSYS2](https://www.msys2.org/). Ces ports permettent d'installer GCC sur Windows, mais il est plus élégant et plus pratique d'utiliser WSL si les applications que vous développez sont destinées à vos études et ne seront pas distribuées à des utilisateurs Windows.

Si vous souhaitez compiler sous Windows avec un compilateur Windows, vous pouvez utiliser [Visual Studio](https://visualstudio.microsoft.com/) qu'il ne faut pas confondre avec Visual Studio Code. Visual Studio est un IDE complet qui inclut un compilateur C/C++. Ce compilateur nommé `cl` est un compilateur propriétaire de Microsoft. Il est inclus dans Visual Studio et n'est pas disponible séparément. Ce compilateur ne respecte pas toujours le standard C (ISO/IEC 9899) mais couvre presque en totalité la norme C++ (ISO/IEC 14882).

## Cycle de compilation

La compilation requiert plusieurs étapes :

1. **Prétraitement** : Le préprocesseur remplace les macros par leur définition, inclut les fichiers d'en-tête, etc.
2. **Compilation** : La compilation converti le code source en assembleur.
3. **Assemblage** : L'assembleur converti le code assembleur en code objet, il lie les bibliothèques statiques et résoud les adresses des fonctions externes.
4. **Édition de liens** : Le lien édite les fichiers objets pour créer un fichier exécutable.

Chacune de ces étapes peut être réalisée séparément. Par exemple, pour compiler un programme en C, il est possible de générer le fichier objet sans l'éditer de liens. Cela permet de gagner du temps lors du développement, car seule la partie modifiée du code est recompilée:

```bash
# Génère un fichier C
$ cat > main.c <<EOF
#include <stdio.h>
int main() {
    printf("Hello, World!\n");
}
EOF

# Étape de préprocesseur
$ gcc -E main.c -o main.i

# Étape de compilation
$ gcc -S main.i -o main.s

# Étape d'assemblage
$ gcc -c main.s -o main.o

# Étape d'édition de liens
$ gcc main.o -o main
```

## Compilation séparée

La compilation séparée est une technique de développement qui consiste à compiler chaque fichier source séparément. Cela permet de réduire le temps de compilation lors du développement, car seuls les fichiers modifiés sont recompilés. Cela permet également de réduire la taille des fichiers objets, car les fonctions non utilisées ne sont pas incluses dans le fichier objet.

```bash
# Génère un fichier C
$ cat > main.c <<EOF
#include "add.h"
#include <stdio.h>
int main() {
    const int a = 2, b = 3;
    printf("Somme de %d+%d = %d\n", a, b, add(a, b));
}
EOF

$ cat > add.h <<EOF
int add(int a, int b);
EOF

$ cat > add.c <<EOF
int add(int a, int b) {
    return a + b;
}
EOF

# Compilation séparée des objets
$ gcc -c main.c -o main.o
$ gcc -c add.c -o add.o

# Édition de liens
$ gcc main.o add.o -o main
```

Notez que si vous avez deux fichiers C mais que vous ne souhaitez pas les compiler séparément, vous pouvez les compiler en une seule commande :

```bash
$ gcc main.c add.c -o main
```

## Options de compilation

`gcc` et `g++` acceptent de nombreuses options de compilation. Les plus courantes sont :

| Option | Description |
|--------|-------------|
| `-c` | Compile le code source en un fichier objet sans l'éditer de liens. |
| `-o` | Spécifie le nom du fichier de sortie. |
| `-I` | Spécifie un répertoire où chercher les fichiers d'en-tête. |
| `-L` | Spécifie un répertoire où chercher les bibliothèques. |
| `-l` | Spécifie une bibliothèque à lier. |
| `-Wall` | Active tous les avertissements. |
| `-Werror` | Traite les avertissements comme des erreurs. |
| `-g` | Inclut des informations de débogage dans le fichier objet. |
| `-O` | Optimise le code. |
| `-std` | Spécifie la norme du langage. |
| `-pedantic` | Respecte strictement la norme. |
| `-D` | Définit une macro. |
| `-U` | Undefine une macro. |
| `-E` | Arrête après l'étape de préprocesseur. |
| `-S` | Arrête après l'étape de compilation. |
| `-v` | Affiche les commandes exécutées par le compilateur. |

Pour compiler un programme avec les optimisations maximales, dans la norme C17, avec tous les avertissements activés et traités comme des erreurs, vous pouvez utiliser la commande suivante :

```bash
$ gcc -std=c17 -O3 -Wall -Werror -pedantic main.c -o main
```

## Bibliothèques

Les bibliothèques sont des fichiers contenant des fonctions et des variables prédéfinies. Il existe deux types de bibliothèques : les bibliothèques **statiques** et les bibliothèques **partagées**. Les bibliothèques statiques ont l'extension `.a` sous POSIX et `.lib` sous Windows. Les bibliothèques partagées ont l'extension `.so` sous POSIX et `.dll` sous Windows.

La bibliothèque standard du langage C est `libc`. Elle est incluse par défaut dans tous les programmes C. Pour inclure une bibliothèque, il suffit de spécifier son nom avec l'option `-l`. Par exmple la bibliothèque `m` contient des fonctions mathématiques. Elle s'appelle `libm` sous Unix/Linux.

Aussi si vous souhaitez calculer le sinus de 3.14, vous pouvez utiliser la fonction `sin` de la bibliothèque `m`, et par conséquent vous devez lier cette bibliothèque à votre programme :

```bash
$ gcc main.c -o main -lm
```

Notez que l'option `-lm` doit être placée après le nom du fichier source. En effet, `gcc` traite les options dans l'ordre où elles apparaissent sur la ligne de commande. Si l'option `-lm` est placée avant le nom du fichier source, `gcc` ne trouvera pas la fonction `sin` et échouera.

Voici quelques bibliothèques couramment utilisées :

| Bibliothèque | Description |
|--------------|-------------|
| `libc` | Bibliothèque standard du langage C. |
| `libm` | Fonctions mathématiques. |
| `libpthread` | Fonctions de threads POSIX. |
| `libcurl` | Client HTTP. |
| `libssl` | Bibliothèque de chiffrement SSL. |
| `libcrypto` | Bibliothèque de chiffrement. |
| `libz` | Compression de données. |
| `libpng` | Traitement d'images PNG. |
| `libsqlite3` | Base de données SQLite. |

## Make

`make` est un outil de gestion de projet qui permet de compiler des programmes C/C++ de manière efficace. `make` utilise un fichier `Makefile` qui contient les règles de compilation.

Le langage Make est très ancien (1976) et n'est pas très lisible aux yeux des débutants. Cependant, il est très puissant et permet de gérer des projets de grande envergure. Voici un exemple de `Makefile` pour compiler un programme en C :

```make
CC=gcc
CFLAGS=-std=c17 -O3 -Wall -Werror -pedantic
LDFLAGS=-lm
EXEC=main
SRCS=$(wildcard *.c)
OBJS=$(SRCS:.c=.o)

all: $(EXEC)

!include $(OBJS:.o=.d)

$(EXEC): $(OBJS)
    $(CC) -o $@ $^ $(LDFLAGS)

%.o: %.c
    $(CC) -o $@ -c $< $(CFLAGS) -MMD -MP

clean:
    $(RM) -f $(OBJS) $(EXEC) $(OBJS:.o=.d)

.PHONY: all clean
```

Make utilise des variables spéciales et l'appel de macros. Les variables sont définies avec `VAR=valeur` et appelées avec `$(VAR)`.

| Variable | Description |
|----------|-------------|
| `CC` | Compilateur (convention) |
| `CFLAGS` | Options de compilation (convention) |
| `LDFLAGS` | Options d'édition de liens (convention) |
| `EXEC` | Nom du fichier exécutable (convention) |
| `SRCS` | Liste des fichiers sources |
| `OBJS` | Liste des fichiers objets |
| `$@` | Nom de la cible |
| `$^` | Liste des dépendances |
| `$<` | Première dépendance |
| `%.o` | Jalon générique pour tous les fichiers en `.o` |
| `$(wildcard *.foo)` | Liste des fichiers `.foo` dans le répertoire courant |
| `$(RM)` | Commande de suppression de fichiers sur le système courant |

Le fonctionnement de Make est en soi assez simple. Des règles sont définies avec `cible: dépendances` et les commandes à exécuter pour générer la cible.

Dans l'exemple ci-dessus, la règle `all` dépend de `$(EXEC)`, autrement dit le fichier `main`. Pour générer `main`, Make recherche une autre règle du même nom. Il trouve `main: $(OBJS)` qui dépend de tous les fichiers objets. Pour générer un fichier objet, Make recherche une autre règle permettant de générer les fichiers objets nécessaires. Il trouve `%.o: %.c` qui dépend de tous les fichiers sources. Une fois les fichiers objets générés, Make peut générer l'exécutable `main`.

Make fonctionne de manière incrémentale. Si un fichier source est modifié, Make ne recompile que les fichiers objets nécessaires. Cela permet de gagner du temps lors du développement. Il se base sur les dates de modification des fichiers pour déterminer si un fichier doit être recompilé ou non.

Ce mode de fonctionnement peut créer des problèmes avec les fichiers d'en-tête. En effet, si un fichier d'en-tête est modifié, Make ne recompile pas les fichiers sources qui incluent ce fichier d'en-tête puisque les fichiers d'en-tête n'apparaissent dans aucune règles. Pour résoudre ce problème, il est possible de générer des fichiers de dépendances avec l'option `-MMD -MP` du compilateur GCC. Ces fichiers de dépendances sont inclus dans le `Makefile` avec l'option `!include $(OBJS:.o=.d)`. Ils contiennent la liste des fichiers d'en-tête inclus par chaque fichier source qui indique par exemple que l'objet `main.o` dépend du fichier `add.h`. Ainsi, si `add.h` est modifié, Make recompile `main.o` et regénère l'exécutable `main`.