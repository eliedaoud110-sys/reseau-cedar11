RÉSEAU CEDAR — VERSION PRÉPARÉE POUR LA SYNCHRONISATION EN LIGNE

Fichiers
- index.html : application
- firebase-config.js : configuration Firebase à compléter

IMPORTANT
Cette version contient le mécanisme de synchronisation pour les annonces, devoirs et documents, mais les messages ne seront partagés entre téléphones qu'après la configuration d'un projet Firebase. La configuration ne peut pas être créée à votre place sans accès à votre compte.

CONFIGURATION (nécessite Internet)
1. Ouvrez https://console.firebase.google.com/ et créez un projet Firebase.
2. Dans Build > Authentication > Sign-in method, activez Anonymous (connexion anonyme).
3. Dans Build > Firestore Database, créez une base de données.
4. Dans Project settings, ajoutez une application Web (</>) et copiez la configuration.
5. Ouvrez firebase-config.js et remplacez les quatre valeurs REMPLACE... par celles de votre projet.
6. Dans Firestore Rules, utilisez temporairement les règles ci-dessous UNIQUEMENT pour un prototype privé de jeu de rôle. Elles permettent à tout utilisateur connecté de lire et écrire ces collections; elles ne conviennent PAS à une vraie école ni à des données personnelles.

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /reseauCedar/shared/{collectionName}/{documentId} {
      allow read, create: if request.auth != null && collectionName in ['annonces', 'devoirs', 'documents'];
      allow update, delete: if false;
    }
  }
}

7. Hébergez le dossier complet sur un hébergeur statique HTTPS (par exemple GitHub Pages). Ne lancez pas seulement le fichier local index.html pour tester Firebase, car les modules web peuvent être bloqués par file://.
8. Ouvrez le même lien sur deux appareils. Publiez une annonce ou un devoir sur le premier; il doit apparaître sur l'autre appareil après la synchronisation.

LIMITES / SÉCURITÉ
- Le choix Élève/Enseignant dans cette démo n'est pas vérifié. Avec ces règles de prototype, toute personne qui ouvre le lien peut publier; ne l'utilisez pas pour une vraie école.
- Les données partagées sont publiques aux personnes qui ont accès au projet via l'application; n'y mettez pas de vrais noms complets, sanctions, données scolaires ou informations sensibles.
- Pour production, il faut des comptes avec mot de passe, rôles attribués par un administrateur et règles Firestore strictes.
- Les éléments existants enregistrés dans l'ancien stockage local ne sont pas automatiquement importés vers Firebase.
