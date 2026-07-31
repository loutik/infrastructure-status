# Infrastructure - Page de statut LoutikCLOUD

![Bannière Loutik](https://raw.githubusercontent.com/loutik/design-assets/main/banniere_loutik.png)

## Contexte

Ce dépôt déploie la page de statut publique de LoutikCLOUD avec [Gatus](https://github.com/TwiN/gatus). L’application surveille les sites, services et hyperviseurs de l’infrastructure, puis expose leur état dans un tableau de bord dédié.

Les contrôles HTTP et ICMP sont configurés dans `config/config.yaml`. Gatus conserve les données d’historique dans une base SQLite montée dans le répertoire de configuration.

## Structure du dépôt

```text
.
├── config/
│   └── config.yaml
├── docker-compose.yaml
├── .gitignore
├── LICENSE.md
└── readme.md
```

- **`config/`** : Configuration de Gatus et stockage persistant de son historique d’incidents.
- **`config/config.yaml`** : Titre du tableau de bord, annonces et endpoints surveillés.
- **`docker-compose.yaml`** : Définition du conteneur Gatus, de son volume, de son redémarrage automatique et de son exposition réseau.
- **`.gitignore`** : Exclusion des fichiers SQLite générés (`data.db`, `data.db-shm` et `data.db-wal`).
- **`LICENSE.md`** : Licence du projet.

## Utilisation de Gatus

### 1. Cloner le dépôt localement

```bash
git clone git@github.com:loutik/infrastructure-status.git
cd infrastructure-status
```

Une installation fonctionnelle de Docker et du plugin Docker Compose est requise.

### 2. Démarrer la page de statut

```bash
docker compose up -d
```

Le service est alors disponible localement à l’adresse [http://127.0.0.1:1025](http://127.0.0.1:1025). Un reverse proxy peut publier cette adresse en HTTPS si la page doit être accessible depuis Internet.

### 3. Vérifier et administrer le service

```bash
docker compose ps

```

Après une modification de `config/config.yaml`, recréez le conteneur pour appliquer la configuration :

```bash
docker compose up -d --force-recreate
```

Pour arrêter le service sans supprimer les données persistées :

```bash
docker compose down
```

## Bonnes pratiques

1. **Conserver l’exposition locale** : le port `1025` est lié à `127.0.0.1` afin d’éviter une exposition directe du conteneur. Utilisez un reverse proxy maîtrisé pour l’accès public.
2. **Protéger la configuration** : ne placez aucun secret dans `config/config.yaml` et limitez les droits d’écriture du répertoire `config/` sur l’hôte.
3. **Préserver les données Gatus** : ne versionnez pas les fichiers SQLite générés et sauvegardez `config/data.db` selon la politique de sauvegarde de l’infrastructure.
4. **Limiter les privilèges** : `NET_RAW` est requis par les sondes ICMP des hyperviseurs. Ne l’accordez pas à d’autres conteneurs et conservez l’image Gatus à jour.
5. **Valider les changements** : contrôlez la configuration et consultez les logs avant de publier une modification.

## 👨‍💻 Mainteneurs

- **Louis MEDO** | [LinkedIn](https://www.linkedin.com/in/louismedo/) | [Portfolio](https://louis.loutik.fr/) | [GitHub](https://github.com/FireToak) | [louis.medo@loutik.fr](mailto:louis.medo@loutik.fr)

-----

<div align="center">
<br>
<small><i>Dernière mise à jour : 31 juillet 2026</i></small>
</div>
