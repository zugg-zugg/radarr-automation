# radarr-automation
This Script interlaces Radarr's requests and automation to download, extract and clean up any rar files leveraging Rclone.

# Requirements
- Pre-configured rclone config file that points to your file source - Such as a seedbox.
- Radarr host that is preconfigured and accessible across your local network/vlan.
- Radarr API Key (RADARR_API_KEY)
- Radarr URL (RADARR_URL)
- RADARR_IMPORT_PATH - This is the local path where files land (mounted into your rclone container as /data)
- RCLONE_DEST_PATH - This is the path *inside* the rclone container that maps to RADARR_IMPORT_PATH
  Note - When using a Docker Container you must define this path variable at the Container level as the storage source is typically not agnostic to one individual container.
- RCLONE_CONTAINER - The name of your rclone Docker container (e.g., binhex-rclone)
- RCLONE_REMOTE - This is tied to your RClone config that needs to be configured prior.

# Process
- Leveraging a proxy that interacts with trackers (Jackett or Prowlerr) Radarr can monitor movies or tv series ((Sonarr).
- When Radarr determines a requested media such as a movie or tv episode is missing it initates a tracker query and begins the download to a preconfigured source. (Ex. Qbittorent on a Seedbox)
- Once the download is completed to the source the script monitors the media folder where this resides and on a pre-defined schedule it leverages RClone to sync the remote and local folder.
- The script downloads all new media or rar files locally and for those compressed it extracts them and cleans up the rar files afterward.
- Pointing your Radarr to monitor this local folder it can then manage the newly obtained media, rename and move it to the appropriate sub folder that is monitored by your Plex or Emby server.

# V3 Updates
+ Added RCLONE_CONFIG_PATH="/config/rclone/config/rclone.conf" (default) Allowing you to specify a unique location for the rclone config itself.

# V2 Updates
+ Cleaned up and added descriptions and spaced out the variables.
+ Added rudimentary throttling of the downloads. V1 did not have any throttling or management in this sense. IF you had 10 things to download it would attempt to download all file at once and cause bottlenecks in most cases.
+ RCLONE_TRANSFERS=1 - Sequential transfers creates a staggered effect. By default the value is 1 but this can be increased accordingly.
+ RCLONE_CHECKERS=2  - Controls the metadata checks - Two seems to be a decent starting point.
+ RCLONE_BWLIMIT=""  - Allows you to place a cap on the bandwidth the downloads can consume. For example "24M" would cap the bandwidth to 24 MiB/s. You can leave this empty for no cap.
