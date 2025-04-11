# clipboard

Procédure de désinstallation (décommissionnement) de l'application DevOps Status (AP24141)
Résumé du contexte
L'application DevOps Status (code AP24141) doit être décommissionnée. Cette application est déployée sur la VM S01VL9957208 (environnement Linux) sous l'utilisateur système outilsdevops_app. Les fichiers applicatifs se trouvent dans le répertoire /applis/24141-tdgg/outilsdevops/app. Les principaux liens internes associés à l'application sont :
Espace de gestion DevOps : lien vers l'espace projet / page de gestion de l'application
RefWeb : lien vers la fiche RefWeb de l'application (AP24141)
Liste des services à arrêter et désactiver
Avant de supprimer l'application, il est nécessaire d'arrêter et de désactiver tous les services liés, afin de garantir qu'aucun composant ne reste actif :
nginx.service
redis.service
auth.service
comm.service
monitoring.service
stream.service
Pour chacun des services ci-dessus, exécutez les commandes suivantes :
bash
Copier
sudo systemctl stop <nom_du_service>
sudo systemctl disable <nom_du_service>
Ces commandes vont arrêter le service puis empêcher son redémarrage automatique au démarrage de la machine.
Suppression des fichiers
Une fois les services arrêtés, supprimez les fichiers de l'application pour libérer l'espace disque. Le répertoire de l'application à supprimer est :
bash
Copier
/applis/24141-tdgg/outilsdevops/app
Assurez-vous qu'aucune donnée importante n'y réside (effectuez une sauvegarde préalable si nécessaire), puis utilisez une commande du type :
bash
Copier
sudo rm -rf /applis/24141-tdgg/outilsdevops/app
pour supprimer définitivement ce dossier et son contenu.
Suppression de l’utilisateur système
Si l'utilisateur Linux dédié à l'application (outilsdevops_app) n'est plus utilisé par d’autres applications, vous pouvez le supprimer afin de nettoyer le système. Avant la suppression, vérifiez qu'aucun processus ne tourne sous cet utilisateur (par exemple avec ps -u outilsdevops_app). Si tout est bon, exécutez :
bash
Copier
sudo userdel -r outilsdevops_app
L'option -r supprime également le répertoire home de l'utilisateur (et son groupe principal s'il n'est plus utilisé ailleurs). Note : Ne supprimez pas cet utilisateur s'il est partagé avec d’autres applications ou services.
Machines virtuelles concernées
Deux machines virtuelles sont associées à cette application :
S01VL9957208 : contient l'application DevOps Status (utilisateur : outilsdevops_app). C’est la VM principale où l'application est installée.
S01VL9957209 : VM actuellement inoccupée ("EMPTY"), initialement prévue pour l'application mais ne contenant aucun composant actif.
Après la désinstallation de l'application, et une fois celle-ci validée, planifiez le décommissionnement de ces deux VMs. Pour S01VL9957208, assurez-vous qu'aucun service ne reste actif après l'arrêt de tous les composants listés précédemment. Ensuite, suivez la procédure interne pour retirer ces machines : mise hors service, suppression ou réaffectation par l'équipe infrastructure, selon les pratiques en vigueur. Veillez à obtenir la validation appropriée avant de supprimer définitivement les VMs.
Mise à jour des outils internes
Une fois l'application désinstallée, mettez à jour les outils internes de suivi pour refléter son arrêt :
Dans l'espace de gestion DevOps lié au projet, marquez l'application comme désactivée ou archivez l'espace/project correspondant, afin d'indiquer qu'elle n'est plus en service.
Mettez à jour le statut de l'application dans RefWeb : passez l'état du projet AP24141 à "Désactivé" (ou l'intitulé approprié signifiant que l'application est retirée), en indiquant la date de fin de service le cas échéant. Ceci assure que la référence interne (RefWeb) reflète la désactivation de l'application.
Checklist finale
Avant de clôturer le décommissionnement, assurez-vous que toutes les étapes suivantes ont été effectuées :
 Services arrêtés et désactivés – Tous les services liés (nginx, redis, auth, comm, monitoring, stream) ont été stoppés et désactivés sur la VM.
 Répertoire applicatif supprimé – Le répertoire de l'application (/applis/24141-tdgg/outilsdevops/app) a été supprimé du système de fichiers.
 Utilisateur système supprimé – L'utilisateur système outilsdevops_app a été supprimé du serveur (s'il n'était pas utilisé ailleurs).
 VMs décommissionnées – Les VMs S01VL9957208 et S01VL9957209 ont été vérifiées (plus aucun processus actif) et leur décommissionnement est planifié/approuvé.
 Espace DevOps archivé – L'espace de gestion DevOps (projet/plateforme interne) de l'application est marqué comme désactivé ou archivé.
 Statut RefWeb à jour – Le statut de l'application dans l'outil RefWeb est mis à jour (application marquée comme désaffectée) et cette mise à jour a été validée.
Une fois tous ces points vérifiés et validés, le processus de désinstallation/décommissionnement de DevOps Status (AP24141) peut être considéré comme terminé. Bonne désinstallation !
