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