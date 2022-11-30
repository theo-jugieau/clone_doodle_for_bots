# Software bots in Software Engineering
L'objectif de cette PR est de présenter différents bots servant à automatiser des tâches sur des dépôts git.

## Renovatebot :
Ce bot sert à mettre à jour les dépendances dans un projet dès qu’il en trouve de nouvelles, il va donc émettre des pulls requests sur le repos(github, gitlab, bitbucket…) et même les auto-merger(si paramétré ainsi).

*Remarque :* l’installation est directe pour github sinon l’application doit être self-hosted.

*Installation sur un repos github :*
Se rendre sur [GitHub Apps - Renovate](https://github.com/apps/renovate) et installer le bot sur le projet ciblé.

Ensuite se rendre sur [GitHub Apps - Renovate](https://github.com/apps/renovate) pour configurer le bot sur un projet spécifique si ce n’est pas déjà fait avec la phase d’installation.

Après l’installation/configuration le bot va émettre une première pull request sur votre dépôt afin de créer un fichier renovate.json qui contiendra les paramètres du bot que vous pourrez alors modifier par la suite si besoin.

Par exemple, on peut ajouter l'option d'auto-merging des PR : ``"automerge": true``,
ou bien programmer les horaires des PR : ``"schedule": ["before 2am"]``

On peut trouver les autres options de configuration [ici](https://docs.renovatebot.com/configuration-options/)


## Créer son propre bot avec Probot :

### Prérequis: npm, node, npx installé sur votre machine

/!\ la doc indique une version de node au moins 10, mais ce n'est pas à jour : il faut une version strictement supérieure à la 10.(nous avons fait avec une 12)

Pour l’installation de node, nous conseillons de passer par le gestionnaire de package nvm. 

* Installation nvm : * ``curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash``

* Installation node/npm : *
``nvm install v12.10.0``
Si pas fait par défaut, activez le node que vous venez d’installer :
``nvm use v12.10.0``

*Installation npx:* ``npm install -g npx``

### Probot :
Exécutez  ``npx create-probot-app my-first-app``
des questions que vous pouvez complètement skip vous sont posées, éviter juste la version ts(pas réussi à la faire marcher chez moi, j’ai pris la version js du coup)
Vous avez alors un répertoire créé, allez y puis exécuter
``npm start``

Se rendre ensuite sur votre navigateur à l’adresse indiquée dans votre navigateur

Ensuite suivre les indications, ajouter votre bot à un repos(cf renovatebot). Une fois fait, redémarrez le serveur: ctrl+C, npm start.
Pour voir si ça fonctionne avec le cas d’exemple des issues: créer un nouvel issue, et si tout se passe bien, le bot doit vous répondre.

C’est donc là que va commencer la partie intéressante qui va être la personnalisation du bot et ça se passe dans le fichier index.js de votre bot.

Vous retrouverez la documentation des webhooks events associés à github [ici](https://docs.github.com/en/developers/webhooks-and-events/webhooks/webhook-events-and-payloads)

