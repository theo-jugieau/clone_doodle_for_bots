# Software bots in Software Engineering
L'objectif de cette PR est de présenter différents bots servant à automatiser des tâches sur des dépôts git.

## Renovatebot :
Ce bot sert à mettre à jour les dépendances dans un projet dès qu’il en trouve de nouvelles, il va donc émettre des pulls requests sur le repos(github, gitlab, bitbucket…) et même les auto-merger(si paramétré ainsi).

*Remarque :* l’installation est directe pour github sinon l’application doit être self-hosted.

*Installation sur un repos github :*
Se rendre sur [GitHub Apps - Renovate](https://github.com/apps/renovate) et installer le bot sur le projet ciblé.
![image](https://user-images.githubusercontent.com/102468174/206925436-9044cadc-d2b4-4637-aafd-331fd9e21f5f.png)
![image](https://user-images.githubusercontent.com/102468174/206925461-ae3d27dd-a352-496c-9018-dc62c41f813d.png)

Ensuite se rendre sur [GitHub Apps - Renovate](https://github.com/apps/renovate) pour configurer le bot sur un projet spécifique si ce n’est pas déjà fait avec la phase d’installation.

Après l’installation/configuration le bot va émettre une première pull request sur votre dépôt afin de créer un fichier renovate.json qui contiendra les paramètres du bot que vous pourrez alors modifier par la suite si besoin.

Par exemple, on peut ajouter l'option d'auto-merging des PR : ``"automerge": true``,
ou bien programmer les horaires des PR : ``"schedule": ["before 2am"]``

On peut trouver les autres options de configuration [ici](https://docs.renovatebot.com/configuration-options/)
* Exemple d'une pull request réalisée par le bot : *
 ![image](https://user-images.githubusercontent.com/102468174/206925292-2d8fbc69-5eac-4a1e-b7ab-1d11fead0f69.png)


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
des questions que vous pouvez ignorer vous sont posées, éviter juste la version ts(pas réussi à la faire marcher chez moi, j’ai pris la version js du coup)
![image](https://user-images.githubusercontent.com/102468174/206924377-a82c3965-64cc-41b9-8320-a7f8582e9cb4.png)

Vous avez alors un répertoire créé, allez y puis exécuter
``npm start``
![image](https://user-images.githubusercontent.com/102468174/206924635-7b25186b-1e4f-4c0a-aaa6-d96ac8a6e1d3.png)

Se rendre ensuite sur votre navigateur à l’adresse indiquée dans votre navigateur
![image](https://user-images.githubusercontent.com/102468174/206924655-3bbe8151-edef-48c6-b1c4-2d3e6d75548d.png)

Ensuite suivre les indications, ajouter votre bot à un repos(cf renovatebot).

![image](https://user-images.githubusercontent.com/102468174/206924699-60e4a52d-0072-4f2c-938f-10d490db67e9.png)
![image](https://user-images.githubusercontent.com/102468174/206924722-9196c710-9c39-411f-8939-87482384df56.png)

Une fois fait, redémarrez le serveur: ctrl+C, npm start.
Pour voir si ça fonctionne avec le cas d’exemple des issues: créer un nouvel issue, et si tout se passe bien, le bot doit vous répondre.

C’est donc là que va commencer la partie intéressante qui va être la personnalisation du bot et ça se passe dans le fichier index.js de votre bot.

Vous retrouverez la documentation des webhooks events associés à github [ici](https://docs.github.com/en/developers/webhooks-and-events/webhooks/webhook-events-and-payloads)
Il y a également de nombreux exemples de bots réalisés avec probot. Par exemple le bot [WIP](https://github.com/wip/app), d'ajouter un marqueur à la PR lorsque l'on ajoute WIP au nom de sa PR permettant ainsi d'éviter qu'elle sa fasse merger par erreur par quelqu'un d'autre. 
On pourrait par exemple imaginer un bot qui applique un linter aux pull requests afin que tout les développeurs aient la même mise en page de code.

## Conclusion sur l'utilisation des bots, et difficultés rencontrés

Nous avons vu qu'il existait des bots pour faire des taches assez variés sur des dépôts git, cependant ils n'ont pas forcément tous leur place. Notamment tout ce qui est bot pour lancer une suite de test n'a pas un énorme intérêt étant donné que d'autres outils comme Jenkins le font très bien.

Pour ce qui est des difficultés rencontrées durant ces TPs, on mentionnera en première ligne les problèmes d'environnements : node et docker. Mais également le fait que seul le propriétaire du dépôt puisse enregistrer les bots rendant le travail en groupe plus compliqué : nous aurions aimé avoir un seul dépôt avec les bots de chacun ainsi au même endroit.
