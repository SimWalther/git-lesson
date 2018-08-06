# Intro à git

## Marche à suivre
- Le document présent est une sorte de mini-cours sur `git` qui essaie de mettre un peu de théorie et un peu de pratique.
- Ceux qui n'ont pas le temps seront peut-être intéressés par le fichier "cheatsheet.md", qui contient un tas de commandes fréquentes et, ma foi, fort utiles.
- Ceux qui ont beaucoup de temps préférerons peut-être le cours OpenClassrooms sur `git` : [lien](https://openclassrooms.com/fr/courses/2342361-gerez-votre-code-avec-git-et-github/2433586-quest-ce-que-versionner-son-code). (Je ne l'ai pas lu mais je leur fais confiance.)
- Pour finir, si vous voulez vous entraîner un max avec les `branches`, je conseille fortement ce site : [lien](https://learngitbranching.js.org/).

## On reprend les bases
`git` est un outil de gestion de versions décentralisé. Ça, on le dit beaucoup, mais on le comprend peu.

- La partie simple signifie qu'il permet de garder plusieurs versions d'un ensemble de fichiers, et de naviguer facilement entre les versions.
- "Décentralisé" peut être un peu plus dur à saisir. Ça signifie que `git` n'utilise pas d'architecture client-serveur. Toutes les versions du dépôt sont indépendantes, y compris celle qui se trouve sur `github` ou `gitlab` ou autre. Il n'y a pas un poste/serveur qui possède une version "maîtresse".
- Il faut donc séparer complètement `git` de ces sites web que l'on utilise pour nous faciliter la vie lors de projets collaboratifs.
- Ça a l'air de rien comme ça, mais cette histoire de décentralisation est une des forces de `git`.

## Initialisation d'un dépôt (repository)
Il y a deux manières principales de créer un dépôt :

- La première dans l'optique de créer un projet en local (auquel on pourra ensuite ajouter une `origin` externe).
- La deuxième dans le but de créer un projet collaboratif dès le départ.

### Initialisation en local
```{sh}
git init
```
- C'est la commande qui initialise un dépôt dans le dossier actuel. Le dossier peut être vide ou avoir déjà des fichiers/dossiers dedans.
- Un message apparaîtra alors disant que la `HEAD` est ambigüe. Il disparaîtra après le premier `commit`. (Nous reparlerons de la `HEAD` plus tard.)
- Si l'on veut ensuite créer un dépôt distant sur `github` (par exemple), il faut avoir fait au moins un `commit`, puis utiliser les commandes suivantes :
```{sh}
git remote add origin git@github.com:Laykel/git-lesson.git
git push -u origin master
```
- La première ajoute une "remote" nommé "origin" à notre dépôt.
- La deuxième pousse notre branche "master" sur la "remote".

### Initialisation sur un site collaboratif
- Là, il faut commencer par créer un dépôt sur le site choisi.
- Une fois cela fait, il nous faudra cloner le dépôt en local pour pouvoir travailler dessus.
- On peut cloner n'importe quel projet public, sans conséquences. Il suffit d'avoir l'URL du dépôt.
- ***1*** Créez le dossier dans lequel vous voulez cloner le cours, déplacez-vous jusqu'à lui dans votre terminal, puis exécutez la commande suivante :

```{sh}
git clone git@github.com:Laykel/git-lesson.git .
```
- L'URL est légèrement modifiée pour gérer facilement les connexions sécurisées, dans le cas des dépôts privés, où seuls les comptes autorisés peuvent cloner le dépôt.
- Le point à la fin dit à `git` de cloner le dépôt dans le dossier courant. Si vous l'omettez, `git` créera un sous-dossier "git-lesson" dans le dossier courant.

## Commandes de bases
Il est temps de repasser sur les commandes qu'on connaît déjà, en les expliquant un peu. *Pour plus de commandes, voir le fichier "cheatsheet.md".*

- `git status` est la plus simple. Elle nous donne une variété d'informations sur l'état du dépôt.
    - Premièrement, elle nous dit sur quelle branche nous nous trouvons. (On reparlera des branches.)
    - Ensuite, s'il y en a, elle nous montre les fichiers qui ont été modifiés depuis le dernier `commit`, et qui n'ont pas été ajoutés à l'index (l'index est là où vont les fichiers lorsqu'on les `add`).
    - Pour finir, s'il y en a, elle nous montre les fichiers qui sont dans l'index et qui n'ont pas encore été `commit`.

- `git log` permet de voir l'historique des `commits`.
    - Un commit est identifié par un hash (SHA1) unique. Nous utiliserons les premiers caractères de ce hash pour revenir à ce `commit` ou faire d'autres manipulations.

- `git diff` montre les changements effectués entre le dernier `commit` et les fichiers "non-committés".

- `git add <file>` permet d'ajouter des fichiers à l'index. On peut en spécifier autant qu'on veut, séparés par des espaces.

- `git commit -m "Commit message"` crée un nouveau `commit` avec les fichiers indexés.
    - Un `commit` contient toutes les modifications apportées aux fichiers.
    - On peut revenir à un précédent `commit`, ou comparer nos fichiers modifiés avec un précédent `commit`.
    - C'est pourquoi il est important de faire des `commits` souvent, et de les décrire précisément.

## Premières manip.
- Actuellement, votre copie du dépôt est "clean", ce qui veut dire qu'aucune modification n'a été faite depuis le dernier `commit`.

- ***2*** Faites une modification à un fichier. Par exemple, modifiez le titre de ce fichier de "Intro" à "Introduction".

- `git status` nous dit maintenant que nous avons une modification non indexée, ce qui signifie qu'elle ne sera pas validée au prochain `commit`.

- ***3*** Ajoutez le fichier à l'index avec `git add README.md` ou `git add .`.

- `git status` nous dit que nous avons un fichier qui sera validé au prochain `commit`. À savoir : Si l'on modifie le fichier indexé avant d'effectuer le `commit`, ces modifications ne seront pas "committées". Les fichiers indexés sont en quelques sortes sauvegardés indépendamment du fichier réel.

- ***4*** Effectuez le `commit` avec `git commit -m "Modification du titre, car ma pédanterie dépasse celle de Luc."`.

- Le commit est alors créé. C'est un objet qui ne changera pas, une brique certaine à laquelle on pourra revenir.

- Cependant, et bien que je respecte votre opiniâtreté sur le sujet des abréviations dans les titres, nous allons inverser votre modification. (Ben oui, après on va `push` le repo, et les autres ne vont pas pouvoir faire la modif si on la laisse !)

- ***5*** On va revenir au commit qui prédate celui de la modification du titre. Pour l'identifier, il nous faut son "hash". Nous allons donc faire un `git log`.

- Votre commit devrait être le plus haut dans la liste, et celui qui nous intéresse est celui juste en-dessous.

- ***6*** Copiez les 4-6 premiers caractères du hash du commit, sortez de `git log` avec `q`, puis exécutez la commande `git revert <hash>` et donnez un message de `commit`.

- ***7*** Pour envoyer les modifications sur le dépôt `github`, il suffit de faire `git push`. En effet, les informations sur la "remote" et les branches qui lui sont liées sont enregistrées dans le fichier de config du repository git (.git/config). Il n'est donc pas nécessaire de spécifier la "remote" ou la branche.
