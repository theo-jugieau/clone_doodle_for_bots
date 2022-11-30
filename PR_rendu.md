# Software bots in Software Engineering
L'objectif de cette PR est de présenter différents bots servant à automatiser des tâches sur des dépôts git.

## Renovatebot :
Ce bot sert à mettre à jour les dépendances dans un projet dès qu’il en trouve de nouvelles, il va donc émettre des pulls requests sur le repos(github, gitlab, bitbucket…) et même les auto-merger(si paramétré ainsi).

*Remarque :* l’installation est directe pour github sinon l’application doit être self-hosted.

*Installation sur un repos github :*
Se rendre sur [GitHub Apps - Renovate](https://github.com/apps/renovate) et installer le bot sur le projet ciblé.

Ensuite se rendre sur [GitHub Apps - Renovate](https://github.com/apps/renovate) pour configurer le bot sur un projet spécifique si ce n’est pas déjà fait avec la phase d’installation.

Après l’installation/configuration le bot va émettre une première pull request sur votre dépôt afin de créer un fichier renovate.json qui contiendra les paramètres du bot que vous pourrez alors modifier par la suite si besoin.
