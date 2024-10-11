## Self-hosted media streaming using Jellyfin + arr stack

This project is a docker-compose stack that includes Jellyfin, Radarr, Sonarr, and Sabnzbd. The stack is used to automate the process of downloading, organizing, and streaming media content.

Why not using torrent? Usenet is a more secure and faster way to download media content. It is also less likely to be monitored by ISPs. Torrent are also known to be less reliable and slower than Usenet but are still a viable option for some users. 

Is this even legal? Downloading copyrighted material is illegal. However, downloading content that is not copyrighted or is in the public domain is legal. It is the user's responsibility to ensure that they are not downloading copyrighted material. Make sure to check the laws in your country before downloading any content.

### Pre-requisites
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- Usenet Provider and Indexer. The following are recommended:
    * [NZBGeek](https://nzbgeek.info/register) - Indexer
    * [Eweka](https://www.eweka.nl/en) - Provider (Main)
    * [NewsDemon](https://www.newsdemon.com/) - Provider (Backup)

If you want to expose the services to the internet under CGNAT, you can use [TailScale](https://tailscale.com/). It is a free service that allows you to create a private network between your devices. You can also use your custom domain by pointing it to the IP address of the machine running the services.

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

### Set up Radarr

1. Open web browser and navigate to `http://localhost:7878`
2. Navigate to `Settings -> Media Management` and add root folders for movies: `/movies`
3. Navigate to `Settings -> Indexers` and add NZBGeek with the following configurations:
    * Add API key from NZBGeek account
    * Categories: `Movies`
    * Tags: Leave blank
4. Navigate to `Settings -> Download` Client and add Sabnzbd with the following configurations:
    * Add API key from Sabnzbd configuration
    * Categories: `movies`
    * Tags: Leave blank

___

### Set up Sonarr

1. Open web browser and navigate to `http://localhost:8989`
2. Navigate to `Settings -> Media Management` and add root folders for TV shows: `/tv`
3. Navigate to `Settings -> Indexers` and add NZBGeek with the following configurations:
    * Add API key from NZBGeek account
    * Categories: `SD`, `HD`
    * Anime Categories: `SD`, `HD`, `Anime`
    * Tags: Leave blank
4. Navigate to `Settings -> Download Client` and add Sabnzbd with the following configurations:
    * Category: `tv`
    * Add API key from Sabnzbd configuration
    * Tags: Leave blank


---

### Set up Jellyseerr

1. Open web browser and navigate to `http://localhost:5055`
2. Choose Jellyfin as the media server
3. Fill in the required details and press `Next`
4. Add radarr and sonarr services. Fill in the required details and press `Next`