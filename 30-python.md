# Python

## Installation de Python

Sous Linux/WSL, l'installation de Python est très simple. Ouvrez un terminal et tapez la commande suivante :

```bash
sudo apt install python3
```

Sous Windows, vous devez choisir la version de Python que vous souhaitez installer. Il est recommandé d'installer la version `3.12` de Python. Utilisez le gestionnaire de paquets `winget` pour installer Python. Ouvrez un terminal `PowerShell` et tapez la commande suivante :

```powershell
winget install -e --id Python.Python.3.12
```

## Configuration des variables d'enviroonement

Selon la méthode utilisée pour installer Python, il est possible que les variables d'environnement ne soient pas configurées automatiquement. Il y a deux entrées à configurer dans la variable `PATH` :

1. Le chemin vers l'exécutable Python. Sous Linux/WSL, le chemin sera déjà configuré (`/usr/bin`). Sous Windows, le chemin diffère selon les époques, les installateurs et les versions.
2. Le chemin vers les paquets locaux installés avec `pip`.

Pour configurer sous Linux/WSL le chemin vers les paquets locaux installés avec `pip`, ouvrez un terminal et tapez la commande suivante :

```bash
export PATH="$HOME/.local/bin:$PATH"
```

Sous Windows, c'est plus compliqué. Selon l'installation de Python vous devez identifier le chemin vers le dossier `Scripts` qui contient les exécutables Python installés avec `pip`.

## Installation des paquets principaux

Installons les paquets les plus couramment utilisés avec Python. Ouvrez un terminal et tapez les commandes suivantes :

```bash
pip install ipython numpy matplotlib pandas jupyterlab black flake8
```