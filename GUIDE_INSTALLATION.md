# GESCO-APC — Guide d'installation et d'utilisation (2026-2027)

> Version du logiciel : 2026 — Compatible portail licences.inofib.com

GESCO-APC est un logiciel de gestion scolaire en PHP/MySQL : établissements, élèves, notes, bulletins, absences, discipline et finances. Une licence annuelle par établissement est obligatoire.

---

## 1. Contenu du pack

```
GESCO-APC-2026-2027.zip
├── installer.bat       ← installation automatique (double-cliquez dessus !)
├── (fichiers de l'application : index.php, home.php, core/, Model/, ...
│    assets/, vendor/, excelReader/, fpdf185/, plugins/, images/, css/, less/)
├── Model/gescoapc_26_27.sql   ← base modèle de démarrage (structure + configuration, SANS données réelles)
└── documentation/
    ├── GUIDE_INSTALLATION.pdf
    ├── GUIDE_INSTALLATION.md
    └── LICENCE_UTILISATION.txt
```

---

## 2. Prérequis techniques

| Élément | Requis | Recommandé |
|---|---|---|
| PHP | 7.4 minimum | **PHP 8.0 / 8.1** |
| Base de données | MySQL 5.7 / MariaDB 10.3 | MySQL 8.0 |
| Serveur web | Apache ou Nginx + PHP-FPM | Apache 2.4 |
| Extensions PHP | pdo_mysql, mysqli, mbstring, openssl, gd, fileinfo, zlib | + curl (télémesure), zip |

> Internet est nécessaire **une fois** au moment de l'activation de la licence (et périodiquement pour la sauvegarde automatique). Le contrôle de validité de la licence fonctionne ensuite **hors-ligne** (signature cryptographique).

---

## 3. Installation

### 3.1 Installation automatique (recommandée — sans manipulation technique)

Pour une personne non informaticienne, tout se fait en **double-cliquant** sur un fichier :

1. Décompressez `GESCO-APC-2026-2027.zip` où vous voulez (Bureau, Téléchargements, clé USB…).
2. Double-cliquez sur **`installer.bat`** — le script vérifie que **Laragon** est installé dans `C:\laragon` avec son dossier `www\`, puis :
   - copie automatiquement le logiciel dans `C:\laragon\www\gescoapc` ;
   - démarre Laragon et attend que **MySQL** fonctionne ;
   - ouvre `http://localhost/gescoapc` dans le navigateur.
3. **Au premier chargement**, la base de données est **créée et remplie automatiquement** à partir du modèle SQL du pack (aucune manipulation MySQL). Rien d'autre à faire.
4. Connectez-vous avec `1275` / `1275` (section 5).

> **Prérequis** : Laragon installé par défaut dans `C:\laragon` (téléchargement : https://laragon.org/download). Si Laragon se trouve ailleurs, copiez simplement le dossier `gescoapc` dans son dossier `www` (`C:\laragon\www\gescoapc`) : le premier chargement créera tout seul la base de données.
>
> Sur un second poste de l'école, répétez la même opération (licence multi-postes).

### 3.2 Installation locale manuelle (WAMP / XAMPP / Laragon)

1. Décompressez `GESCO-APC-2026-2027.zip` dans le dossier du serveur web :
   - XAMPP : `C:\xampp\htdocs\gescoapc`
   - WAMP : `C:\wamp64\www\gescoapc`
   - Laragon : `C:\laragon\www\gescoapc`
2. Lancez Apache et MySQL depuis le panneau de contrôle (XAMPP : démarrer Apache + MySQL).
3. Ouvrez dans un navigateur : `http://localhost/gescoapc`

### 3.3 Installation sur un hébergeur (en ligne)

1. Décompressez le pack dans le dossier racine du site (ex. `public_html/gescoapc`).
2. Créez la base de données et l'utilisateur MySQL depuis l'espace client (cPanel : « Bases de données MySQL »).
3. Reportez les identifiants dans `Model/connexion.php` et `Model/checkdb.php` :
   - `Model/connexion.php` (ligne ~11) : `new PDO('mysql:host=...;dbname=gescoapc_26_27;...', 'UTILISATEUR', 'MOT_DE_PASSE')`
   - `Model/connexion.php` (ligne ~53) : `mysqli_connect("hote", "UTILISATEUR", "MOT_DE_PASSE", "gescoapc_26_27")`
   - `Model/checkdb.php` (ligne ~44) : `new mysqli("hote", "UTILISATEUR", "MOT_DE_PASSE")`
4. Ouvrez l'application dans un navigateur.

---

## 4. Création de la base de données

### 4.1 Nouvel établissement (première installation)

> **Aucune action nécessaire en installation locale** : la base `gescoapc_26_27` est **créée et remplie automatiquement au premier chargement** de l'application (section 3.1). L'import manuel ci-dessous ne sert qu'à titre de *secours* si l'import automatique a rencontré un problème.

1. Ouvrez **phpMyAdmin** (`http://localhost/phpmyadmin` en local, ou l'outil de l'hébergeur).
2. Créez une base nommée **`gescoapc_26_27`** (encodage `utf8mb4_unicode_ci`).
3. Ouvrez l'onglet **Importer** et chargez le fichier **`Model/gescoapc_26_27.sql`**.
4. Validez. L'application est prête.

> Le fichier `Model/gescoapc_26_27.sql` est une **base vierge de démarrage** : structure complète + configuration générique (matières, niveaux, sessions, un compte admin « 1275 / 1275 » et un établissement « ETABLISSEMENT » par défaut). **Aucun élève, personnel ou donnée réelle n'y figure** : vous enregistrez vos propres données après connexion. Bonne pratique : supprimez le fichier SQL du serveur une fois la base importée.

### 4.2 Établissement utilisant déjà une version antérieure

1. Conservez votre base existante intacte (ex. `gescoapc_25_26`).
2. Installez le nouveau pack par-dessus (ne PAS importer le fichier SQL : l'application crée automatiquement la base de la nouvelle année en copiant l'ancienne au premier lancement).

---

## 5. Première connexion

1. Ouvrez l'application (`http://localhost/gescoapc`).
2. Dans le menu déroulant, l'année scolaire **2026/2027** est sélectionnée.
3. Identifiez-vous avec le compte d'administration de démarrage :

   | Identifiant | Mot de passe | Rôle |
   |---|---|---|
   | `1275` | `1275` | Administration (admin) |

4. **Changez immédiatement ce mot de passe** (menu réglages / gestion des utilisateurs) et créez vos propres comptes (secrétariat `sg`, enseignants `ens`, caisse `caisse`, …).
5. Configurez le nom de votre établissement (menu **Réglages généraux** → informations de l'établissement).

> ⚠️ **Important avant l'achat de la licence** : la licence est liée au **nom exact de l'établissement** et à l'**année scolaire**. Fixez ces valeurs dans le logiciel **avant** de payer ; toute modification manuelle ultérieure du nom ou de l'année verrouille l'application (protection anti-fraude). Le support INOFIB peut cependant débloquer en cas de changement légitime (fusion, changement de nom) — fournissez l'`install_id`.

---

## 6. Activation de la licence

1. Récupérez votre **`install_id`** : il s'affiche sur la page **Activation** du logiciel (bouton « Activation » après connexion, ou page `activation.php`).
2. Cliquez sur **« Payer la licence annuelle sur INOFIB »** : vous êtes redirigé vers le portail `licences.inofib.com` avec l'établissement, l'année et l'`install_id` pré-remplis.
3. Réglez via **Assiin Pay** (Mobile Money). Une **clé de licence** vous est envoyée **par email**.
4. Collez cette clé dans le logiciel, page **Activation** (Étape 2), puis **Valider l'activation**.

La licence est alors active pour l'année scolaire, pour le nombre de postes payés.

- **Tarifs** : grille par effectif (tranches) disponible sur `licences.inofib.com`.
- **Renouvellement** : chaque année scolaire (nouvelle session), une nouvelle licence est nécessaire. L'application vous redirigera automatiquement vers le portail.
- **Changement / panne d'ordinateur** : sur `licences.inofib.com` utilisez « Récupérer ma clé » ; ressaisissez l'`install_id` d'origine.
- **Sauvegarde automatique** : vos données (référentiels) sont envoyées de façon périodique et chiffrée sur le portail INOFIB (contrôle effectif + restauration/audit).

### Messages de verrouillage fréquents

- **« Licence arrivée à expiration »** : renouvelez sur le portail.
- **« Verrouillage anti-fraude : année scolaire »** : vous utilisez la base d'une autre année que celle de la clé ; connectez-vous avec la bonne session dans la liste déroulante.
- **« Verrouillage anti-fraude : nom d'établissement »** : le nom local a été modifié depuis l'émission de la clé ; récupérez une nouvelle clé sur le portail ou contactez le support.

---

## 7. Modules de l'application

- **Élèves / inscriptions** : création des élèves, matricules, effectifs, niveaux et classes.
- **Notes & évaluations** : saisie par matière et par classe, contrôle continu, moyennes, classements.
- **Bulletins** : génération PDF (fpdf185) par trimestre.
- **Absences & discipline** : suivi et sanction (points).
- **Finances** : pensions, versements, gestion de la caisse.
- **Utilisateurs & rôles** : admin, secrétariat, enseignant, caisse, proviseur, … selon les permissions.
- **Import/Export Excel** : gestion des effectifs via PhpSpreadsheet.

---

## 8. Sauvegarde et restauration

- **Exporter la base** : phpMyAdmin → base `gescoapc_<année>` → **Exporter** (SQL).
- **Restaurer** : phpMyAdmin → **Importer** votre fichier SQL de sauvegarde.
- La plateforme INOFIB conserve par ailleurs des sauvegardes périodiques des référentiels (télémesure).

---

## 9. Nouvelle année scolaire

1. À la rentrée, l'application détecte automatiquement la nouvelle session et crée la base correspondante en copiant la structure et les référentiels de l'année précédente (élèves, classes, matières…).
2. La **licence de l'année en cours reste valable** jusqu'au 31/08.
3. Pour la nouvelle session, reconnectez-vous avec la nouvelle année dans la liste : l'application vous fera payer et activer la **nouvelle licence** (elle prend effet à la date de début de la rentrée).

---

## 10. Sécurité et bonnes pratiques

1. **Changez les mots de passe des comptes par défaut** dès la première connexion.
2. **Supprimez le fichier SQL** `Model/gescoapc_26_27.sql` du serveur une fois importé.
3. En ligne : protégez le dossier par HTTPS et créez un utilisateur MySQL dédié (pas l'administrateur global).
4. Effectuez régulièrement une **exportation de la base** (au minimum en fin de chaque mois).
5. Ne modifiez pas le nom de l'établissement ni l'année scolaire sans l'accord du support (licence liée à ces valeurs).

---

## 11. Dépannage

| Symptôme | Cause probable | Solution |
|---|---|---|
| `Erreur de connexion base de données` | Identifiants MySQL incorrects dans `Model/connexion.php` / `Model/checkdb.php` | Vérifier hôte, utilisateur, mot de passe |
| Bascule vers `activation.php` au login | Licence absente, expirée ou incompatible | Payer / coller la clé (section 6) |
| « Base introuvable » au premier lancement | `Model/gescoapc_26_27.sql` non importé | Importer dans `gescoapc_26_27` (section 4.1) |
| Import Excel « classe non trouvée » | Extension `fileinfo` ou `zip` manquante | Activer l'extension dans PHP |
| Page blanche | Version PHP trop ancienne ou extension manquante | PHP 8.0+, activer pdo_mysql, mysqli, mbstring, openssl |
| La licence ne se valide pas (signature) | Clé collée incomplète | Reprendre la clé intégrale de l'email |

---

## 12. Support

- Portail : `https://licences.inofib.com`
- Pour toute question : utilisez le formulaire du portail ou contactez votre revendeur INOFIB.

© INOFIB — GESCO-APC est distribué sous licence annuelle. La distribution, copie ou revente sans autorisation est interdite (cf. `documentation/LICENCE_UTILISATION.txt`).