# Instructions de Build (macOS)

Suivez les indications ci-dessous pour compiler Electron sur macOS.

## Prérequis

- macOS >= 10.11.6
- [Xcode](https://developer.apple.com/technologies/tools/) >= 8.2.1
- [node.js](https://nodejs.org) (externe)
- Python 2.7 avec le support de TLS 1.2

## Python

Veuillez vous assurer que votre système et votre version de Python supporte au moins le TLS 1.2. Cela dépend à la fois de votre version de macOS et Python. Pour un test rapide, lancez:

```sh
$ python ./script/tls.py
```

Si le script vous retourne que votre configuration utilise un protocole de sécurité obsolète, vous pouvez soit mettre à jour macOS vers High Sierra ou installer une nouvelle version de Python 2.7.x. Pour mettre à jour Python, utilisez [Homebrew](https://brew.sh/):

```sh
$ brew install python@2 && brew link python@2 --force
```

Si vous utilisez le Python fournit par Homebrew, vous devez également installer les modules Python suivants:

- [pyobjc](https://pythonhosted.org/pyobjc/install.html)

## SDK macOS

Si vous développez Electron et ne prévoyez pas de redistribuer votre version d'Electron vous pouvez sauter cette section.

Pour que certaines fonctionnalités (par exemple pinch-zoom) fonctionnent correctement, vous devez cibler le SDK macOS 10.10.

Les builds officielles d'Electron sont compilés avec [Xcode 8.2.1](http://adcdownload.apple.com/Developer_Tools/Xcode_8.2.1/Xcode_8.2.1.xip), qui ne contient pas le SDK 10.10 par défaut. Pour l’obtenir, commencez par télécharger et monter le DMG [Xcode 6.4](http://developer.apple.com/devcenter/download.action?path=/Developer_Tools/Xcode_6.4/Xcode_6.4.dmg).

Puis, en supposant que le DMG Xcode 6.4 a été monté dans `/Volumes/Xcode` et que votre installation Xcode 8.2.1 est dans `/Applications/Xcode.app`, exécutez :

```sh
cp -r /Volumes/Xcode/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.10.sdk /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/
```

Vous devrez aussi activer Xcode pour compiler avec le SDK 10.10 :

- Ouvrir `/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Info.plist`
- Mettre la valeur `MinimumSDKVersion` à `10.10`
- Sauvegarder le fichier

## Obtenir le Code

```sh
$ git clone https://github.com/electron/electron
```

## Amorçage

Le script d'amorçage téléchargera toutes les dépendances nécessaires et créera les fichiers de compilation. Nous utilisons [ninja](https://ninja-build.org/) pour compiler Electron, donc il n'y a aucun projet Xcode généré.

```sh
$ cd electron
$ ./script/bootstrap.py -v
```

Si votre éditeur supporte un serveur de langage basé sur une [base de données de compilation JSON](http://clang.llvm.org/docs/JSONCompilationDatabase.html), vous pouvez la générer:

```sh
$ ./script/build.py --compdb
```

## Compilation

Compiler une version `Release` et une version `Debug` :

```sh
$ ./script/build.py
```

Vous pouvez également compiler seulement une version `Debug` :

```sh
$ ./script/build.py -c D
```

Une fois la compilation terminée, vous trouverez `Electron.app` dans `out/D`.

## Support 32 bit

Electron ne peut être compiler qu'en 64 bit sur macOS et il n'est pas prévu d'avoir un support 32 bit sur macOS dans le futur.

## Nettoyage

Pour nettoyer les fichiers de build :

```sh
$ npm run clean
```

Pour nettoyer uniquement les répertoires `out` et `dist` :

```sh
$ npm run clean-build
```

**Remarque :** Les deux commandes de nettoyage requière l’exécution de `bootstrap`.

## Tests

Voir [Build System Overview : Tests](build-system-overview.md#tests)