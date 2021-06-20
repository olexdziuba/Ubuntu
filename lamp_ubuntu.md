Installation LAMP pour installation de CMS (Joomla et Wordpress)
----------------------------------------------------------------

![](images/php.jpg)

Dans [l'article précédent](https://www.google.com/url?q=https://olexdziuba.github.io/two_siteweb_UBUNTU_apache/&sa=D&source=editors&ust=1624161500272000&usg=AOvVaw3doRP1Kr7hkcyon4eYbLWZ), nous avons mis deux sites Web sur un serveur en utilisant Apache. Pour le test, nous avons utilisé deux sites Web simples créés en html. Cependant, maintenant presque personne n'utilise de tels sites. Il est beaucoup plus facile de vieillir les sites Web en utilisant le CMS. Alors installons un site sur WordPress et un autre sur Joomla

Pour installer CMS comme WordPress ou Joomla il faut installer LAMP (Linux, Apache, MySQL, PHP), on a déjà installé Apache. Il reste à installer  MySQL et PHP et faire configuration d'apache. On va commencer d’installation MySQL

#### Installation de MySQL

*olex@ubuntu:\~\$ sudo apt install mysql-server*

![](images/php4.png)

Configurer le plugin de validation de mot de passe

*olex@ubuntu:\~\$ sudo mysql\_secure\_installation*

![](images/php19.png)

Pour toutes les questions suivantes, entrez simplement Y et appuyez sur la touche ENTER pour sélectionner les paramètres par défaut. Cela supprimera certains utilisateurs et bases de données de test, désactivera l'accès à distance avec le compte utilisateur root et appliquera immédiatement toutes les modifications à MySQL.

![](images/php17.png)

#### Installation de PHP 

PHP est un composant qui traitera le code pour rendre le contenu dynamique. Nous installerons également des packages auxiliaires afin que le code PHP puisse fonctionner avec notre serveur Apache, ainsi qu'accéder à la base de données MySQL.

*olex@ubuntu:\~\$ sudo apt install php libapache2-mod-php php-mysql*

![](images/php15.png)

Pour que notre serveur Web préfère les fichiers PHP, nous allons donc configurer Apache pour rechercher d'abord les fichiers index.php.

On va corriger fichier:

*olex@ubuntu:\~\$ sudo nano /etc/apache2/mods-enabled/dir.conf*

Il faut déplacer le fichier d'index PHP *index.php* à la première place après la spécification *DirectoryIndex* comme ceci

![](images/php16.png)

Après cela, nous devons redémarrer Apache pour appliquer les modifications.

![](images/php7.png)

vérifier l'état du service apache2 à l'aide de *systemctl*

![](images/php13.png)

Pour verification PHP on va créer *info.php* dans */var/www/html/*

![](images/php6.png)

On va ajouter:

\*<?php*

*phpinfo();*

*?\>*

![](images/php3.png)

Apres on va aller:

*http://IP_serveur/info.php*

Et voilà, serveur PHP fonctionne:

![](images/php11.png)



#### Installation PHPMyAdmin

L’installation de PHPMyAdmin n’est pas du tout obligatoire mais cela nous facilitera la vie.

*olex@ubuntu:\~\$ sudo apt install phpmyadmin*

![](images/php14.png)

Choisissez d’utiliser *PHPMyAdmin* pour un serveur Apache.

![](images/php2.png)

Lorsqu'on lui demande si *dbconfig-common* doit être utilisé pour configurer la base de données, sélectionnez *Oui*.

![](images/php5.png)

Pour le mot de passe root, il s’agit de celui que vous aviez utilisé pour MySQL.

Vous pouvez choisir *password*. Il vaut mieux utiliser un mot de passe fort !

![](images/php9.png)

Le processus d'installation ajoutera le fichier de configuration phpMyAdmin au répertoire */etc/apache2/-enabled/*, où il sera lu automatiquement. Tout ce que vous avez à faire est d'activer explicitement l'extension PHP *mbstring*, ce qui peut être fait avec la commande suivante :

*olex@ubuntu:\~\$ sudo phpenmod mbstring*

![](images/php10.png)

Après il faut redémarrer Apache

*olex@ubuntu:\~\$ sudo systemctl restart apache2*

Configuration de l'accès par mot de passe pour le compte root dans MySQL

Pour vous connecter à phpMyAdmin avec l'utilisateur root MySQL, vous devez changer la méthode d'authentification de auth\_socket à mysql\_native\_password si vous ne l'avez pas déjà fait. Pour ce faire, ouvrez une ligne de commande MySQL via un terminal :

*olex@ubuntu:\~\$ sudo mysql*

Après il faut vérifier quelle méthode d'authentification vos comptes d'utilisateur MySQL utilisent avec la commande suivante :

*mysql\> SELECT user,authentication\_string,plugin,host FROM mysql.user;*

![](images/php1.png)

L'utilisateur root utilise en fait la méthode d'authentification en utilisant le plugin auth\_socket. Pour configurer l'authentification par mot de passe pour le compte root, exécutez la commande ALTER USER suivante.

*mysql\> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql\_native\_password BY 'MyPass';*

Pour appliquer les modification, exécutez la commande FLUSH PRIVILEGES:

*mysql\> FLUSH PRIVILEGES;*

![](images/php12.png)

Vérifiez les méthodes d'authentification pour confirmer que l'utilisateur root n'utilise plus le plug-in auth\_socket pour l'authentification :

*mysql\> SELECT user,authentication\_string,plugin,host FROM mysql.user;*

![](images/php8.png)



Vous pouvez désormais accéder à l'interface web en tapant le nom de domaine ou l'adresse IP publique de votre serveur et en ajoutant */phpmyadmin* :

![](images/php18.png)

J'ai utilisé un utilisateur root mais il vaut mieux créer un utilisateur séparé!

Vous devez également configurer le fichier *.htaccess* pour augmenter la sécurité

Le serveur est maintenant prêt à installer le CMS.
