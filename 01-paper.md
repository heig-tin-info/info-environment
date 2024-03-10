---
exports:
  - format: docx
  - format: pdf
    template: volcanica
    article_type: Report
---
# L'environnement Windows/Linux pour l'ingénieur

**Authors:** Prof. Yves Chevallier
**Affiliations:** Haute École d'Ingénierie et de Gestion du Canton de Vaud
**License:** CC-BY

**Abstract**

Vous êtes étudiant et vous êtes perdus avec l'utilisation de Python, LaTeX, Git, etc. sous Windows, Linux ou encore Docker Ce document est fait pour vous. Il vous guidera dans l'installation et l'utilisation de ces outils. L'objectif est de comprendre les avantages et les inconvénients de chaque outil et de vous permettre de les utiliser de manière efficace.

## Préambule

Les applications utilisées typiquement par un ingénieur aujourd'hui sont Python, Git, GCC, LaTeX ou même Docker. Ces applications ont un point commun, c'est qu'elles ont d'abord été écrites pour un environnement **POSIX** (Unix).

**POSIX** est une norme internationale (IEEE 1003) qui définit l'interface de programmation d'un système d'exploitation. Elle est basée sur UNIX. Elle est utilisée pour les systèmes d'exploitation de type UNIX. Windows n'est pas un système qui respecte cette norme ce qui complique l'utilisation de ces applications.

Afin de pouvoir porter Python ou Git sous Windows, il a fallu ajouter une couche d'abstraction pour rendre compatible le monde Linux avec le monde Windows. Historiquement le projet [Cygwin](https://en.wikipedia.org/wiki/Cygwin) né en 1995 a été le premier à proposer une telle couche. Il s'agissait d'un environnement POSIX pour Windows muni d'un terminal, d'un gestionnaire de paquets et d'une bibliothèque d'émulation POSIX. Les outils en ligne de commande type `ls`, `cat` ou même `grep` étaient proposés. Néanmoins, Cygwin n'était pas parfait, il nécessitait son propre environnement de travail et n'était pas bien intégré à Windows. Le projet [MSYS](https://en.wikipedia.org/wiki/MSYS) a été créé en 2003 pour pallier à ces problèmes. Il s'agissait d'une couche d'abstraction POSIX pour Windows qui se voulait plus légère. Au lieu de compiler des applications Linux qui devaient impérativement être lancées sous Cygwin, MSYS intégrait la couche d'abstraction dans les applications elles-mêmes, ces dernières étaient compilées en `.exe` et pouvaient être lancées directement depuis l'explorateur Windows. MSYS a été un franc succès et a été intégré dans le projet [MinGW](https://en.wikipedia.org/wiki/MinGW) (Minimalist GNU for Windows) qui est un portage de GCC pour Windows.

Lorsque vous installez `Git` sous Windows et que vous visitez l'emplacement d'installation (`C:\Program Files\Git`), vous verrez des dossiers aux noms compatibles `POSIX` comme `bin`, `etc`, `lib`, `usr`, etc. Le dossier `bin` qui contient les exécutables contient `bash` qui n'est rien d'autre que le terminal utilisé sous Linux. La raison est que `Git` ou même `Python` sont des outils avant tout développés pour les environnements Unix/Linux.

Le choix de l'environnement de travail est donc compliqué. Faut-il travailler sous Windows avec les limitations que le portage des applications Linux implique ou faut-il travailler sous Linux directement ? Un ingénieur reste  aujourd'hui attaché au monde Windows car il dépend de logiciels comme `SolidWorks` ou `Altium Designer` qui ne sont pas disponibles sous Linux. Il est donc nécessaire de trouver un compromis.

En 2016, Microsoft a annoncé le support de Linux dans Windows 10. Il s'agissait d'une couche d'abstraction qui permettait de faire tourner des applications Linux directement sous Windows. Cette couche d'abstraction s'appelle [Windows Subsystem for Linux](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux) (WSL). Elle est basée sur une technologie de virtualisation de conteneurs. WSL a été un franc succès et il a très vite été adopté par les développeurs Web. Il fut de surcroît proposé comme une alternative à Cygwin et MSYS.

WSL a été amélioré au fil des années et en 2019, Microsoft a annoncé WSL 2. WSL 2 est basé sur une technologie de virtualisation de type 2 (hyperviseur) et non plus de type 1 (noyau Linux). WSL 2 est donc plus performant que WSL 1. Il est possible de faire tourner un serveur web ou même un serveur de base de données directement sous WSL 2. WSL 2 est maintenant une alternative crédible à Linux.

Il rend possible de travailler sous Windows et de faire tourner des applications Linux directement sous Windows.

Le choix donné aux ingénieurs est donc :

1. **Choix facile mais source d'incohérences**: Travailler exclusivement sous Windows et installer `Python`, `Git`, `LaTeX` sous Windows. L'inconvénient est que chacune de ses applications ne profitent pas d'une unité de travail commune. A force d'installer des applications, vous aurez dans votre système plusieurs installation de Python, plusieurs exécutables Git ce qui peut vite devenir compliqué à gérer.
2. **Choix plus difficile mais offrant davantage de flexibilité**: Travailler sous WSL 2 et de faire tourner `Python`, `Git`, `LaTeX` sous Linux. L'avantage est que vous aurez une unité de travail commune. Vous pourrez installer des applications Linux directement depuis le gestionnaire de paquets de votre distribution Linux. Néanmoins vous devrez vous familiariser avec la ligne de commande Linux.

## Le terminal

Historiquement sous Windows, le terminal était une application graphique appelée `cmd`. Elle n'a pas évoluée depuis Windows NT. Son interface est limitée à un nombre fini de caractères par ligne et ne supportait que quelques couleurs. Elle ne supportait pas les caractères Unicode et ne supportait pas les raccourcis clavier comme `Ctrl+C` ou `Ctrl+V`.

Heursement Windows a évolué et propose PowerShell. PowerShell est un terminal plus moderne qui supporte les couleurs, les raccourcis clavier, les caractères Unicode, etc. PowerShell est un terminal plus puissant que `cmd`.

L'interface du terminal était également rudimentaire (pas d'onglets, pas de séparateurs, etc.). Heureusement Windows propose depuis 2019 [Windows Terminal](https://en.wikipedia.org/wiki/Windows_Terminal). Windows Terminal est un terminal moderne multi-onglets qui supporte plusieurs terminaux (cmd, PowerShell, WSL, etc.). S'il n'est pas installé vous pouvez le faire via le [Microsoft Store](https://www.microsoft.com/fr-ch/p/windows-terminal/9n0dx20hk701).

```{figure} images/cmd.png
:alt: cmd.exe
:width: 500px
:align: center
:figclass: margin

Interface de cmd.exe dans Windows Terminal
```

```{figure} images/powershell.png
:alt: powershell.exe
:width: 500px
:align: center
:figclass: margin

Interface de PowerShell dans Windows Terminal
```

```{figure} images/bash.png
:alt: Ubuntu
:width: 500px
:align: center
:figclass: margin

Interface de Ubuntu dans Windows Terminal
```

## Git

Git est un outil de gestion de versions. Il a été inventé par Linus Torvalds en 2005 pour gérer le développement du noyau Linux qui contient des millions de lignes de code devant être modifiées par des centaines de milliers de développeurs. Git a été conçu pour être rapide, efficace et pour gérer des projets de toutes tailles. Il est aujourd'hui le système de gestion de version le plus utilisé au monde.

Git est la suite logique des outils comme `CVS` ou `Subversion` qui étaient utilisés pour gérer des projets de développement de logiciels.

Git est avant tout un outil en ligne de commande. Son utilisation passe par la commande `git` suivi d'une sous-commande (`clone`, `pull`, `push`, `commit`, etc.). Il est possible de l'utiliser avec une interface graphique mais l'interface en ligne de commande est plus puissante, plus rapide et plus flexible.

Un dépôt (référentiel) Git est un dossier qui contient un dossier caché `.git`. Ce dossier contient l'historique des modifications du projet. Ne supprimez pas ce dossier, vous perdriez l'historique de votre projet. Lorsque vous faites des *commit* (sauvegarde incrémentationnelle), Git enregistre les modifications dans ce dossier caché et lorsque vous faites un *push* (envoi des modifications sur un serveur distant), Git envoie les modifications à un serveur distant (GitHub, GitLab, Bitbucket, etc.).

Par conséquent, l'utilisation de Git est intrincèquement liée à l'utilisation d'un serveur distant. Il est possible de travailler en local mais l'intérêt de Git est de pouvoir travailler en équipe. Il est possible de travailler sur une branche (version parallèle du projet) et de fusionner les modifications avec la branche principale (master).

Ceci implique de comprendre comment Git communique avec un serveur distant. Notons qu'il est possible de travailler avec plusieurs serveurs distants. Ils se configure avec la commande `git remote`. Deux protocoles de communication existent : `SSH ` et `HTTPS`. Le protocole `SSH` est plus sécurisé que `HTTPS` mais nécessite l'échange de clés cryptographiques. Le protocole `HTTPS` est plus simple à mettre en place mais demande un mot de passe ou un jeton d'authentification à chaque communication avec le serveur distant. La solution recommandée est d'utiliser `SSH`.

## Variables d'environnement

Que vous soyez sous POSIX ou Windows, votre système d'exploitation dispose de variables d'environnement. Il s'agit de variables qui sont accessibles par toutes les applications. Elles sont utilisées pour stocker des informations comme le chemin d'accès à un exécutable, le nom de l'utilisateur, le répertoire de travail, etc.

La variable la plus importante est `PATH`. Elle contient une liste de chemins d'accès aux exécutables. Lorsque vous tapez une commande dans un terminal, le système d'exploitation parcourt les chemins d'accès de la variable `PATH` pour trouver l'exécutable correspondant à la commande. Si vous avez installé Python, Git, LaTeX, etc. dans des répertoires différents, il est nécessaire de les ajouter à la variable `PATH`. Parfois les installateurs le font automatiquement, parfois non. Il est donc nécessaire de vérifier manuellement.

Une variable d'environnement n'est propagée à un processus que si ce dernier est lancé après la modification de la variable. Si vous modifiez la variable `PATH` les processus déjà lancés ne verront pas la modification. Il est nécessaire de fermer le terminal et de le rouvrir (relancer Visual Studio Code, votre terminal, etc.).

Parfois si vous installez plusieurs version d'un même logiciel comme `Python` vous pourriez avoir plusieurs variables d'environnement qui pointent vers des versions différentes de Python. C'est une source de confusion et c'est un problème fréquent sous Windows. Vous pouvez vérifier quel est le chemin d'accès à un exécutable avec la commande `where` sous Windows et `which` sous Linux.

- Linux/WSL/MacOS

  ```bash
  $ which python
  /usr/bin/python
  ```

- Windows
  - `cmd`

    ```cmd
    PS C:\> where python
    C:\Python39\python.exe
    ```

  - `PowerShell`

    ```powershell
    PS C:\> Get-Command python
    ```

## Installation

### Git

#### Installation de Git

Pour installer Git sous Windows, le plus simple est d'utiliser le nouveau gestionnaire de paquet de Windows appelé `winget`. Ouvrez un terminal `PowerShell` et tapez la commande suivante issue de [Winget Git/Git](https://winget.run/pkg/Git/Git)

```powershell
winget install -e --id Git.Git
```

Sous Linux, et particulièrement Ubuntu, Git est probablement déjà installé par défaut. Si ce n'est pas le cas, avec Ubuntu ouvrez un terminal Bash et tapez la commande suivante :

```bash
sudo apt install git
```

> [!NOTE]
> Selon la distribution Linux que vous utilisez, les gestionnaires de paquets n'ont pas le même nom. Sous Fedora, le gestionnaire de paquets est `dnf` et sous Arch Linux, le gestionnaire de paquets est `pacman`. Néanmoins la commande reste très similaire.

Sous MacOS, Git est probablement déjà installé. Si ce n'est pas le cas, vous pouvez installer Git avec [Homebrew](https://brew.sh) en tapant la commande suivante dans un terminal :

```bash
brew install git
```

#### Configuration de Git

##### Utilisateur et adresse e-mail

La première chose à faire après avoir installé Git est de le configurer. Ouvrez un terminal et tapez les commandes suivantes en veillant bien à remplacer `John Doe` par votre nom et `john.doe@acme.com` par votre adresse e-mail.

```bash
git config --global user.name "John Doe"
git config --global user.email john.doe@acme.com
```

Ces informations sont utilisées pour chaque *commit* que vous ferez. Elles sont importantes car elles permettent de savoir qui a fait une modification dans le projet. Ne vous trompez pas dans votre nom ou votre adresse e-mail, il est difficile de changer ces informations une fois qu'elles ont été enregistrées dans un *commit*, et surtout si elle ont été envoyées sur un serveur distant.

##### Méthode de fusion (*merge*)

Lorsque vous utilisez la commande `git pull` pour récupérer les modifications d'un serveur distant, Git peut être amené à fusionner des modifications. Il est possible de choisir la méthode de fusion. Les deux méthodes les plus courantes sont `merge` et `rebase`. La méthode `merge` est la méthode par défaut. La méthode `rebase` est plus avancée et est utilisée pour réécrire l'historique du projet. Elle est plus propre mais elle peut être source de problèmes si elle est mal utilisée. Git s'attend à que vous configuriez laquelle des deux méthodes vous préférez. Vous pouvez le faire avec la commande suivante :

Pour le rebase :

```bash
git config --global pull.rebase true
```

Pour le merge :

```bash
git config --global pull.rebase false
```

> [!NOTE]
> La méthode `rebase` est plus propre mais elle peut être source de problèmes si elle est mal utilisée. Elle est recommandée pour les projets personnels mais pas pour les projets en équipe. En cas de doute privilégiez la méthode `merge`.

##### Affichage de l'historique des commits (*log*)

Lorsque vous utilisez la commande `git log` pour afficher l'historique des *commits*, Git affiche les *commits* dans un format compact. Il est possible de personnaliser l'affichage de l'historique des *commits*. Vous pouvez le faire avec la commande suivante :

```bash
git config --global alias.lg "log -n30 --boundary --graph --pretty=format:'%C(bold blue)%h%C(bold green)%<|(20)% ar%C(reset)%C(white)% s %C(dim white)-% an%C(reset)%C(auto)% d%C(bold red)% N' --abbrev-commit --date=relative"
```

Dès lors vous utiliserez le raccourcis `lg` pour afficher l'historique des *commits* (`git lg`).

Pour afficher la date en format ISO 8601, vous pouvez utiliser la commande suivante :

```bash
git config --global alias.lga "log -n30 --boundary --graph --pretty=format:'%C(bold blue)%h%C(bold green)%<|(20)% ai%C(reset)%C(white)% s %C(dim white)-% an%C(reset)%C(auto)% d%C(bold red)' --abbrev-commit --date=iso"
```

##### Configuration SSH

Comme nous l'avons vu, Git peut communiquer avec un serveur distant en utilisant le protocole `SSH`. Pour cela, il est nécessaire de configurer une clé cryptographique.

La première chose à faire est de vérifier si vous avez déjà une clé SSH. Ouvrez un terminal et tapez la commande suivante :

```bash
$ ls -al ~/.ssh/*.pub
-rw-r--r-- 1 ycr ycr 393 2023-09-06 08:38 /home/ycr/.ssh/id_rsa.pub
```

Le fichier `id_rsa.pub` est votre clé publique. Si vous ne voyez pas ce fichier, c'est que vous n'avez pas de clé SSH. Vous pouvez en générer une avec la commande suivante :

```bash
ssh-keygen
```

Sous Windows, la commande est la même mais vous devez lancer `Git Bash` pour l'exécuter. Vous pouvez lancer `Git Bash` en tapant `Git Bash` dans le menu de recherche de Windows.

##### Configuration de GitHub

Si vous utilisez GitHub, il est nécessaire de configurer votre clé SSH dans votre compte GitHub. Pour cela, ouvrez un terminal et tapez la commande suivante :

```bash
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC+yNp7af6zI8NINIFX1aRj+nzKksZ6XzBSkgA/iuPpYIGz5SSZOkwkvN0DnX8J42DcuEK/mnu3+f9Wh746823gxhXqtj+7Wv9z9DJ9O9qrsYlnxIMipoqepE/Xt+jE5Yv8ullIdsvZdzY611R5DFwrVswslz9OdmpH6nWCmnY/cGZva79ngdcvJLKFk++fl+Be1xshWt24svawRH7Fdxn8VyUKmP2Twy6iMo3MT9xGe5leV1CiTXfkzLYntNV50/dtzQN+pwcwRBdXBP9FdO9+IzieY6bUGttT6t2VcWoK6jFF+i94Chl/FeGvRU1X/QzSP3SYT2biNRNmznSIa2VD ycr@heig-vd
```

Copiez la clé publique et collez-la dans votre compte GitHub. Pour cela, ouvrez votre navigateur et allez sur [GitHub](https://github.com). Connectez-vous à votre compte et cliquez sur votre photo de profil en haut à droite. Cliquez sur `Settings` puis sur `SSH and GPG keys`. Cliquez sur `New SSH key` et collez votre clé publique dans le champ `Key`. Donnez un nom à votre clé et cliquez sur `Add SSH key`.

### Python

#### Installation de Python

Sous Linux/WSL, l'installation de Python est très simple. Ouvrez un terminal et tapez la commande suivante :

```bash
sudo apt install python3
```

Sous Windows, vous devez choisir la version de Python que vous souhaitez installer. Il est recommandé d'installer la version `3.12` de Python. Utilisez le gestionnaire de paquets `winget` pour installer Python. Ouvrez un terminal `PowerShell` et tapez la commande suivante :

```powershell
winget install -e --id Python.Python.3.12
```

#### Configuration des variables d'enviroonement

Selon la méthode utilisée pour installer Python, il est possible que les variables d'environnement ne soient pas configurées automatiquement. Il y a deux entrées à configurer dans la variable `PATH` :

1. Le chemin vers l'exécutable Python. Sous Linux/WSL, le chemin sera déjà configuré (`/usr/bin`). Sous Windows, le chemin diffère selon les époques, les installateurs et les versions.
2. Le chemin vers les paquets locaux installés avec `pip`.

Pour configurer sous Linux/WSL le chemin vers les paquets locaux installés avec `pip`, ouvrez un terminal et tapez la commande suivante :

```bash
export PATH="$HOME/.local/bin:$PATH"
```

Sous Windows, c'est plus compliqué. Selon l'installation de Python vous devez identifier le chemin vers le dossier `Scripts` qui contient les exécutables Python installés avec `pip`.

#### Installation des paquets principaux

Installons les paquets les plus couramment utilisés avec Python. Ouvrez un terminal et tapez les commandes suivantes :

```bash
pip install ipython numpy matplotlib pandas jupyterlab black flake8
```

### LaTeX

Sous Linux/WSL, le plus simple est d'installer LaTeX avec le gestionnaire de paquets de votre distribution Linux. Ouvrez un terminal et tapez la commande suivante :

```bash
sudo apt install texlive-full latexmk
```

Sous Windows c'est plus compliqué. Il existe plusieurs distributions LaTeX pour Windows. La plus courante est [MiKTeX](https://miktex.org). Téléchargez l'installateur et suivez les instructions.

## Commandes principales

### Git

| Commande | Description |
| --- | --- |
| `git init` | Initialise un dépôt Git dans le répertoire courant |
| `git clone <address>` | Clone un dépôt distant dans le répertoire courant |
| `git status` | Affiche l'état du dépôt |
| `git add <file>` | Ajoute des fichiers à l'index |
| `git commit -am "message"` | Enregistre les modifications dans l'historique du dépôt |
| `git pull` | Récupère les modifications du serveur distant |
| `git push` | Envoie les modifications sur le serveur distant |
| `git log` | Affiche l'historique des *commits* |
| `git lg` | Affiche l'historique des *commits* de manière plus lisible |

### GCC

| Commande | Description |
| --- | --- |
| `gcc` | Compilateur C |
| `g++` | Compilateur C++ |
| `make` | Gestionnaire de compilation |

#### Compiler un programme C

```bash
gcc -o hello hello.c
```

#### Compiler un programme C++

```bash
g++ -o hello hello.cpp
```

#### Compiler plusieurs fichiers C

```bash
gcc -o hello main.c functions.c
```

#### Compiler séparément les fichiers C

```bash
gcc -c functions.c
gcc -c main.c
gcc -o hello main.o functions.o
```

### Linux/WSL

| Commande | Description |
| --- | --- |
| `ls` | Liste les fichiers du répertoire courant |
| `cd` | Change de répertoire |
| `pwd` | Affiche le répertoire courant |
| `cat` | Affiche le contenu d'un fichier |
| `less` | Affiche le contenu d'un fichier page par page |
| `grep` | Recherche une chaîne de caractères dans un fichier |
| `find` | Recherche un fichier dans un répertoire |
| `man` | Affiche le manuel d'une commande |
| `which` | Affiche le chemin d'accès à un exécutable |

#### Afficher les fichiers du répertoire courant

```bash
ls -al # Par noms
ls -lt # Par date de modification
ls -lh # En format lisible
```

### Python

| Commande | Description |
| --- | --- |
| `python` | Lance l'interpréteur Python |
| `pip` | Gestionnaire de paquets Python |
| `ipython` | Lance l'interpréteur IPython |
| `jupyter lab` | Lance l'environnement Jupyter Lab |

### LaTeX

| Commande | Description |
| --- | --- |
| `latexmk -xelatex` | Compile un document LaTeX |
