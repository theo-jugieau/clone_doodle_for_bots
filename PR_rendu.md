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

## Rultor, a DevOps team assistant

Rultor permet d'automatiser les opérations git comme merge, deploy et release avec une interface chat-bot intégré dans les commentaires d'une pull request.
Rultor permet d'isoler le script de déploiement dans son propre environnement virtuel en utilisant Docker. Ce qui permet de réduire les états externes qui pourrait entrainer des erreurs lors des tests.

Lors d'une pull request, lorsqu'on va lui demander, Rultor va récupérer la branche master et y appliquer les modifications proposés. Il va ensuite tout exécuter sur son Docker et si tout se passe bien sans erreurs, il va merge la branche dans la branche master. Cela permet de réduire les risques que les développeurs cassent la branche master en faisant une pull request contenant des erreurs. Grâce à cela, les développeurs ont moins peur de faire des erreurs et augmente leur productivité. 
Rultor est très facilement utilisable grâce à des mots clés à mettre dans les commentaires. 

### Comment mettre en place Rultor ? 

Dans un premier temps il faut récupérer rultor dans le [Marketplace de Git](https://github.com/marketplace/rultor-com) et récupérer le fichier rultor.yml de ce [dépôt](https://github.com/yegor256/rultor) qui sera à mettre à la racine du projet.

Tout d'abord il faut créer un serveur Ubuntu accessible via l'adresse `b4.rultor.com`, Rultor s'y connectera via SSH. Il faut ensuite installer Docker Engine dessus.
```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Il faut ensuite configurer le serveur pour donner les droits d'accès à Rultor.
```
$ apt-get install -y bc
$ groupadd docker
$ adduser rultor
$ gpasswd -a rultor docker
$ echo 'rultor ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers
$ mkdir /home/rultor/.ssh
$ cat > /home/rultor/.ssh/authorized_keys
$ chown rultor:rultor -R /home/rultor/.ssh
$ chmod 600 /home/rultor/.ssh/authorized_keys
```

Puis, nous devons créer l'image que le docker va devoir utiliser.

On créé un conteneur `sudo docker run -i -t ubuntu /bin/bash`, on installe les packages désirés, et on génere son image `sudo docker commit 215d2696e8ad rultor/beta`.
Il est aussi possible de récupérer l'[image par défaut de Rultor](https://hub.docker.com/r/yegor256/rultor-image/).

Nous envoyons ensuite l'image sur le hub Docker `sudo docker push rultor/beta` et nous modifions le fichier rultor.yml : 
```
docker:
  image: rultor/beta
```

Enfin, pour exécuter Rultor il suffit de faire une pull request sur la branche master du projet git et écrire `@rultor merge` en commentaire.
