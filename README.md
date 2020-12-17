# arch-html-bootstrap

## Importer le code bootstrap dans mon architecture.

(tuto réaliser avec la version 4.5 de Bootstrap)

Pour commencer télécharger le repo de bootstrap:
  - https://github.com/twbs/bootstrap/archive/main.zip

## Pour le css

Ensuite extraire le .zip
  - Copier le dossier "scss" et le renommer en "bootstrap" (./public/css/sass/bootstrap)

Ensuite nous allons appeler l'index de bootstrap dans notre index.sass (./public/css/sass/index.sass)
```
// import du Scss Bootstrap (css)
@import bootstrap/bootstrap
    
// import de Mon Sass (css)
@import partial/style
```

Il nous manque plus qu'a compiler notre sass et ressortir un style.css qui sera appeler par notre html

commande sass (depuis la racine /arch-html-bootstrap):

```
sass --watch ./public/css/sass/index.sass:./public/css/style.css
```

## Pour le JS
Pour importer le JS nous allons directement copié le contenue des liens CDN.

Exemple:
  - Jquery: 
```
<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
```
  - récupérer le lien du cdn (https://code.jquery.com/jquery-3.5.1.slim.min.js) dans notre exemple
  - rendez-vous dessus et copié l'intégralité du code
  - ensuite creer un fichier dans votre architecture pour acceuillir (./public/js/jquery.js)
  - il nous manque plus qu'a éditer l'import du js dans notre html
 ```
 <script src="public/js/jquery.js"></script>
 ```

Voila il ne vous reste plus qu'a faire de même avec popper et le js de bootstrap.
Pour récupérer les cdn à jour de bootstrap rendez-vous ici:
  - https://getbootstrap.com/
  
(Attention la version 5 de bootstrap n'importe plus jquery et à compiler le bundle de bootstrap avec popper)

Attention tout les chemins vers les fichiers ou dossier sont unique à ce projet, à vous de l'adapter au votre !

## Mettre en production un site html sur un serveur ubuntu (vps ovh) avec nginx

Pré-requis:
  - un serveur avec accès ssh
  - nginx installer sur le serveur
  - un projet réaliser en html
  - un nom de domaine ou sous-domaine qui pointe sur notre serveur (domaine.com -> 1.1.1.1)

Étape n°1)
  - Envoyer notre projet sur le serveur (plusieurs technique sont possible )
    - Sois avec github ou en SSH avec la commande "scp"

En SSH:
Se connecter au serveur:
```
ssh user@1.1.1.1
(user doit etre remplacer par votre user / 1.1.1.1 doit etre remplacer par l'ip de votre serveur)
(le mot de passe de votre utilisateur pour accèder au serveur)
(puis creer le dossier qui va acceuillir votre projet)

mkdir ~/website
(~/ : sert à récupérer la racine de notre utilisateur à ne pas confondre avec / : qui est la racine de notre machine)
```

Uploader mon projet depuis mon pc vers le serveur:
```
cd arch-html-bootstrap
cd ..
scp -r -p ./arch-html-bootstrap user@1.1.1.1:website/
```

explication de la commande:
scp: La commande "scp" nous permet de copier des fichiers/dossiers d'un serveur à un autre.
-r: pour le mode récurcive (on prend tout les enfants avec)
-p: préserve les dates de modification, d’accès, et les modes des anciens fichiers.
./arch-html-bootstrap: fichier/dossier d'entrée à copier
user@1.1.1.1:website/: serveur de destination du fichier/dossier (1.1.1.1 doit etre remplacer par l'ip de votre serveur) (website/ est bien le dossier de reception dans votre serveur !!! vous devez d'abord le creer sur votre serveur au cas ou ce n'est pas fait)

Ou avec github:
  - Installer git sur notre serveur et le configuré avec github (clef ssh, variable global, ...)

Se connecter sur notre serveur:

```
ssh user@1.1.1.1
mkdir website
cd website
git clone <votre repo git>
```

Étape n°2)
  - Déplacer mon repo dans les dossiers web de notre server (/var/www)

```
cd ~/website
sudo mkdir /var/www/website
sudo mv arch-html-bootstrap /var/www/website
```

Étape n°3)
  - Configurer nginx et faire notre redirection

Pour cela nous allons devoir créé un fichier de config dans (/etc/nginx/sites-enabled) et creer notre config:
```
cd /etc/nginx/sites-enabled
sudo touch monsite.conf
sudo vim monsite.conf
```
puis coller la config suivante dans vim:
```
server {
  # notre serveur est bien à l'écoute du port 80
	listen 80;
  # ici 127.0.0.1:80
	listen [::]:80;

  # ici on récupère la redirection de notre domaine faite par notre service de dns
  # remplacer bien domain.com par votre domaine
	server_name domain.com www.domain.com;

  # ici on indique le chemin vers la ou est stocker notre projet sur le serveur
	root /var/www/website/arch-html-bootstrap;
  # et on lui indique de lire notre index
	index index.html;

  # en cas d'url non trouver afficher erreur 404
	location / {
		try_files $uri $uri/ =404;
	}
}
```

Voila il ne nous reste plus qu'a redémarrer nginx sur notre serveur
```
sudo systemctl restart nginx
```

Rendez-vous sur votre navigateur et essayer de rejoindre votre domaine.

Erreur connu:
  - configuration de github: https://git-scm.com/book/fr/v2/D%C3%A9marrage-rapide-Param%C3%A9trage-%C3%A0-la-premi%C3%A8re-utilisation-de-Git
  - Config nginx (default server): https://stackoverflow.com/questions/30973774/nginx-duplicate-default-server-error
  - ma config (/etc/nginx/nginx.conf):
  ```
  http {
    ...
    	include /etc/nginx/conf.d/*.conf;
	    include /etc/nginx/sites-enabled/*.conf;
  }
  ```
    - j'ai juste modifier ces 2 ligne include a la fin du module http

En + (avancé):
  - Déployer sur github en une ligne de commande: https://medium.com/@stadja/comment-d%C3%A9ployer-sur-un-serveur-distant-en-une-commande-git-pas-%C3%A0-pas-bdcf5aaf172d
