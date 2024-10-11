## Self-hosted media streaming using Jellyfin + arr stack

### Pre-requisites
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- Usenet Provider and Indexer. The following are recommended:
    * [NZBGeek](https://nzbgeek.info/register) - Indexer
    * [Eweka](https://www.eweka.nl/en) - Provider (Main)
    * [NewsDemon](https://www.newsdemon.com/) - Provider (Backup)

___

### Setup Sabnzbd

1. Open web browser and navigate to `http://localhost:8080`
2. Fill in the required credentials using the Provider's account details
3. Navigate to `Config -> Folders`
    * Set `Temporary Download Folder` to `/incomplete-downloads`
    * Set `Complete Download Folder` to `/downloads`
4. Navigate to `Config -> Categories`
    * Set `movies` category folder to `/downloads` and press `Save`
    * Set `tv` category folder to `/downloads` and press `Save`

    Note: _Delete other categories if present (Not mandatory)_
5. Navigate to `Config -> General`
    * Copy the API key from `API Key` field and save it for later use

---

### Set up Radarr and Sonarr

#### Radarr:

1. Navigate to `Settings -> Media Management` and add root folders for movies: `/movies`
2. Navigate to `Settings -> Indexers` and add NZBGeek with the following configurations:
    * Add API key from NZBGeek account
    * Categories: `Movies`
    * Tags: Leave blank
3. Navigate to `Settings -> Download` Client and add Sabnzbd with the following configurations:
    * Add API key from Sabnzbd configuration
    * Categories: `movies`
    * Tags: Leave blank

#### Sonarr:

1. Navigate to `Settings -> Media Management` and add root folders for TV shows: `/tv`
2. Navigate to `Settings -> Indexers` and add NZBGeek with the following configurations:
    * Add API key from NZBGeek account
    * Categories: `SD`, `HD`
    * Anime Categories: `SD`, `HD`, `Anime`
    * Tags: Leave blank
3. Navigate to `Settings -> Download Client` and add Sabnzbd with the following configurations:
    * Category: `tv`
    * Add API key from Sabnzbd configuration
    * Tags: Leave blank
