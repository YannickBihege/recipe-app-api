# Backend d'Application Django

Backend d'application Django conteneurisé avec support de base de données PostgreSQL et déploiement automatisé via GitHub Actions.

## Technologies Utilisées

- Python 3.9
- Django 3.2.4
- Django REST Framework 3.12.4
- PostgreSQL 13
- Docker & Docker Compose
- GitHub Actions pour CI/CD
- Flake8 pour la qualité du code

## Structure du Projet

```
.
├── app/                    # Racine de l'application Django
├── .github/               
│   └── workflows/         # Workflows GitHub Actions
├── requirements/
│   ├── requirements.txt   # Dépendances de production
│   └── requirements.dev.txt # Dépendances de développement
├── .dockerignore
├── .gitignore
├── docker-compose.yml
├── Dockerfile
└── README.md
```

## Prérequis

- Docker et Docker Compose installés
- Git installé
- Compte GitHub (pour le déploiement)

## Configuration de l'Environnement de Développement

1. Cloner le dépôt :
```bash
git clone <url-du-depot>
cd <repertoire-du-projet>
```

2. Créer les fichiers d'environnement :
```bash
cp .env.example .env
```

3. Construire et lancer les conteneurs Docker :
```bash
docker-compose up --build
```

4. Exécuter les migrations :
```bash
docker-compose exec app python manage.py migrate
```

5. Créer un superutilisateur :
```bash
docker-compose exec app python manage.py createsuperuser
```

L'application sera accessible à l'adresse http://localhost:8000

## Variables d'Environnement

Variables d'environnement clés utilisées dans le projet :

```
DB_HOST=db
DB_NAME=devdb
DB_USER=devuser
DB_PASS=changeme
```

## Configuration Docker

Le projet utilise deux services principaux :

### Service Application
- Construit à partir de l'image Python 3.9 Alpine
- Exécute l'application Django
- Configuré pour le développement avec montage de volumes
- Expose le port 8000

### Service Base de Données
- PostgreSQL 13 Alpine
- Volume persistant pour le stockage des données
- Configuré avec variables d'environnement

## Exécution des Tests

Pour exécuter la suite de tests :

```bash
docker-compose exec app python manage.py test
```

Pour vérifier la qualité du code avec Flake8 :

```bash
docker-compose exec app flake8
```

## Déploiement

L'application utilise GitHub Actions pour le déploiement automatisé. Le workflow comprend :

1. Vérification de la Qualité du Code
2. Tests Unitaires
3. Construction et Publication de l'Image Docker
4. Déploiement en Production

### Workflow GitHub Actions

Le workflow est déclenché sur :
- Push sur la branche principale
- Pull requests vers la branche principale

Fichier de configuration : `.github/workflows/checks.yml`

## Directives de Développement

### Qualité du Code

Le projet utilise Flake8 pour garantir la qualité du code. Configuration dans `.flake8` :

```ini
[flake8]
exclude =
    migrations,
    __pycache__,
    manage.py,
    settings.py
max-line-length = 100
```

### Apporter des Modifications

1. Créer une nouvelle branche :
```bash
git checkout -b feature/nom-de-votre-fonctionnalite
```

2. Effectuer vos modifications et s'assurer que les tests passent :
```bash
docker-compose exec app python manage.py test
docker-compose exec app flake8
```

3. Commiter vos modifications :
```bash
git add .
git commit -m "Description des modifications"
git push origin feature/nom-de-votre-fonctionnalite
```

4. Créer une Pull Request sur GitHub

## Documentation de l'API

La documentation de l'API est disponible à `/api/docs/` lors de l'exécution de l'application.

## Gestion de la Base de Données

### Création des Migrations

```bash
docker-compose exec app python manage.py makemigrations
```

### Application des Migrations

```bash
docker-compose exec app python manage.py migrate
```

### Sauvegarde des Données

```bash
docker-compose exec db pg_dump -U devuser devdb > backup.sql
```

## Résolution des Problèmes

Problèmes courants et solutions :

1. Problèmes de connexion à la base de données :
   - Vérifier que le service PostgreSQL est en cours d'exécution
   - Vérifier les variables d'environnement
   - Vérifier la connectivité réseau entre les conteneurs

2. Problèmes de permissions :
   - Vérifier la propriété des fichiers dans les volumes montés
   - Vérifier les permissions utilisateur dans les conteneurs

3. Problèmes de migration :
   - Réinitialiser les migrations si nécessaire
   - Vérifier les conflits de migration

## Contribution

1. Forker le dépôt
2. Créer votre branche de fonctionnalité
3. Commiter vos modifications
4. Pousser vers la branche
5. Créer une Pull Request

## Licence

[Votre Licence Ici]

## Contact

Mainteneur : bihegeyannick

## Remerciements

- Documentation Django
- Documentation Docker
- Documentation GitHub Actions