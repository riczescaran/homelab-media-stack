## Self-hosted media streaming using Jellyfin + arr stack

This project is a docker-compose stack that includes Jellyfin, Radarr, Sonarr, and Sabnzbd. The stack is used to automate the process of downloading, organizing, and streaming media content.

<b>Why you made this?</b> 

It is a fun project to work on and can be useful for people who want to have their own media server. Most of the information is available online but it is scattered and can be confusing for beginners. This project aims to simplify the process of setting up a media server using Docker. I am not responsible for any illegal activities that may be done using this project :)

<b>Why not using torrent?</b>

Usenet is a more secure and faster way to download media content. It is also less likely to be monitored by ISPs. Torrent are also known to be less reliable and slower than Usenet but are still a viable option for some users. 

<b>Is this even legal?</b>

Downloading copyrighted material is illegal. However, downloading content that is not copyrighted or is in the public domain is legal. It is the user's responsibility to ensure that they are not downloading copyrighted material. Make sure to check the laws in your country before downloading any content.


### Pre-requisites
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- Usenet Provider and Indexer. The following are the services I have used:
    * [NZBGeek](https://nzbgeek.info/register) <b>(Indexer)</b>. Why I chose this service:
        * Affordable pricing
        * Offers a wide range of content
        * Offers lifetime membership
    * [Eweka](https://www.eweka.nl/en) <b>(Provider)</b>. Why I chose this service:
        * Affordable pricing
        * Unlimited speed
        * Unlimited downloads
        * Offers high retention period

    Note: _You can use any Usenet provider and indexer of your choice. It usually depends on your budget and the content you want to download._

### Optional

- If you want to expose the services to the internet under CGNAT, you can use [TailScale](https://tailscale.com/). It is a free service that allows you to create a private network between your devices. You can also use your custom domain by pointing it to the IP address of the machine running the services.

- If you want to monitor your arr stack services using mobile app, you can use [LunaSea](https://lunasea.app/). It is a free app that allows you to monitor and manage your services from your mobile device.

___

## Running the services

1. Create a new docker network using the following command:
    ```bash
    docker network create proxy_network
    ```
2. Run the following command to start the services:
    ```bash
    docker-compose up -d
    ```
3. Check if all the services are running using the following command:
    ```bash
    docker-compose ps
    ```
4. If all the services are running, you can access them using the following URLs:
    * Jellyfin: `http://localhost:8096`
    * Jellyseerr: `http://localhost:5055`
    * Radarr: `http://localhost:7878`
    * Sonarr: `http://localhost:8989`
    * Prowlarr: `http://localhost:9696`
    * Sabnzbd: `http://localhost:8080`
    * Homepage: `http://localhost:3000`

---

## Setup Sabnzbd

1. Open web browser and navigate to `http://localhost:8080`
2. Fill in the required credentials using your preferred Provider. In my case, I have used Eweka and NewsDemon.
3. Navigate to `Config -> Folders`
    * Set `Temporary Download Folder` to `/incomplete-downloads`
    * Set `Complete Download Folder` to `/downloads`
4. Navigate to `Config -> Categories`
    * Create a new category `radarr` and press `Save`
    * Create a new category `sonarr` and press `Save`

    Note: _Delete other categories if present (Not mandatory)_
5. Navigate to `Config -> General`
    * Copy the API key from `API Key` field and save it for later use

---

## Set up Radarr

1. Open web browser and navigate to `http://localhost:7878`
2. Navigate to `Settings -> Media Management` and add root folders for movies: `/movies`
3. Navigate to `Settings -> Download Client` and add Sabnzbd with the following configurations:
    * Add API key from Sabnzbd configuration
    * Categories: `radarr`
    * Tags: Leave blank
4. Navigate to `Settings -> General` to get the API key for Radarr and save it for setting up Prowlarr

___

## Set up Sonarr

1. Open web browser and navigate to `http://localhost:8989`
2. Navigate to `Settings -> Media Management` and add root folders for TV shows: `/tv`
3. Navigate to `Settings -> Download Client` and add Sabnzbd with the following configurations:
    * Category: `sonarr`
    * Add API key from Sabnzbd configuration
    * Tags: Leave blank
4. Navigate to `Settings -> General` to get the API key for Sonarr and save it for setting up Prowlarr


---

## Setup Prowlarr

1. Open web browser and navigate to `http://localhost:9696`
2. Navigate to `Settings -> Apps` and add the following applications:
    * Radarr
        * Name: Radarr
        * Sync Level: Full Sync
        * Tags: Leave blank
        * Prowlar Server: `http://prowlarr:9696`
        * Radarr Server: `http://radarr:7878`
        * API Key: API key from Radarr
        * Press `Test` to check if the connection is successful and press `Save`
        
    * Sonarr
        * Name: Sonarr
        * Sync Level: Full Sync
        * Tags: Leave blank
        * Prowlar Server: `http://prowlarr:9696`
        * Sonarr Server: `http://sonarr:8989`
        * API Key: API key from Sonarr
        * Press `Test` to check if the connection is successful and press `Save`
3. Navigate to `Indexers` and add your preferred indexers.  In my case, I have added NZBGeek.

    Note: _Fill in the required details and press `Test` to check if the connection is successful and press `Save`_

4. To verify that the indexers are synced with Radarr and Sonarr, go to `Settings -> Indexers` in both services. You should see the indexers you added in Prowlarr listed there. See the screenshots below for reference:

    ![Radarr_Prowlarr](/screenshots/prowlarr/radarr_prowlarr.png)
    ![Sonarr_Prowlarr](/screenshots/prowlarr/sonarr_prowlarr.png)

---

### Set up Jellyseerr

1. Open web browser and navigate to `http://localhost:5055`
2. Choose Jellyfin as the media server
3. Fill in the required details and press `Next`
4. Add radarr and sonarr services. Fill in the required details and press `Next`

---

### Setup Homepage

1. Open web browser and navigate to `http://localhost:3000` to see if homepage is running. See the screenshot below for reference:

    ![Homepage](/screenshots/homepage/homepage.png)
2. To enable widgets, modify the `_data\homepage\services.yaml` file and set the correct IP addresses and API keys for the services. You can also add or remove services as needed. See the screenshot below for reference:

    ![Homepage_Widgets](/screenshots/homepage/homepage_widgets.png)

--- 

## Screenshots

### Jellyfin
![Jellyfin](/screenshots/jellyfin.png)

### Jellyseerr
![Jellyseerr](/screenshots/jellyseerr.png)

### Radarr
![Radarr](/screenshots/radarr.png)

### Sonarr
![Sonarr](/screenshots/sonarr.png)

### Prowlarr
![Prowlarr](/screenshots/prowlarr.png)

### Sabnzbd
![Sabnzbd](/screenshots/sabnzbd.png)
