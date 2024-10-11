## Setting up Radarr and Sonarr

### Pre-requisites
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- Usenet Provider and Indexer. The following are recommended:
    * [NZBGeek](https://nzbgeek.info/register) - Indexer
    * [Eweka](https://www.eweka.nl/en) - Provider (Main)
    * [NewsDemon](https://www.newsdemon.com/) - Provider (Backup)

### Setting up Radarr and Sonarr

* Setup Download Client
    * Radarr:
        * NZBGeek (Indexer)
            * Categories: `Movies`
            * Tags: Leave blank
        * Sabnzbd (Download Client)
            * Categories: `movies`
            * Tags: Leave blank
    * Sonarr:
        * NZBGeek (Indexer)
            * Categories: `SD`, `HD`
            * Anime Categories: `SD`, `HD`, `Anime`
            * Tags: Leave blank
        * Sabnzbd (Download Client)
            * Category: `tv`
            * Tags: Leave blank

* Setup Media Management root folders (Settings -> Media Management)
    * Radarr:
        * Movie Folder: `/movies`
    * Sonarr:
        * TV Folder: `/tv`