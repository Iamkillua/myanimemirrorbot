# Anime Mirror Bot

**Anime Mirror Bot** is a Telegram Bot writen in Python based on Breakdowns Slam mirroring bot for mirroring files on the Internet to our beloved Google Drive.

# Features supported:

## Additional Features
- Mirroring Uptobox.com links to Google Drive (Uptobox account must be premium)
- Get detailed info about replied media
- Nyaa.si and Sukebei Torrent search
- Added support for Piratebay, EZTV, 1337x and YTS Torrent search
- Speedtest with picture results
- Limiting Torrent size support
- Sudo with Database support
- Check Heroku dynos stats
- Custom image support
- Racaty.net support
- Shell and Executor
- Stickers module
- Anime Manga and Charecter search from anilist.co

## From Original Repos
- Mirroring direct download links, Torrent, and Telegram files to Google Drive
- Mirroring Mega.nz links to Google Drive (If your Mega account not premium, it will limit 4-5gb/day)
- Copy files from someone's Drive to your Drive (Using Autorclone)
- Download/upload progress, speeds and ETAs
- Mirror all Youtube-dl supported links
- Docker support
- Uploading to Team Drive
- Index Link support
- Service Account support
- Delete files from Drive
- Shortener support
- Custom Filename (Only for url, Telegram files and Youtube-dl. Not for Mega links and Magnet/Torrents)
- Extracting password protected files, using custom filename and download from password protected index links see these examples:
- Extract these filetypes and uploads to Google Drive
```
ZIP, RAR, TAR, 7z, ISO, WIM, CAB, GZIP, BZIP2, 
APM, ARJ, CHM, CPIO, CramFS, DEB, DMG, FAT, 
HFS, LZH, LZMA, LZMA2, MBR, MSI, MSLZ, NSIS, 
NTFS, RPM, SquashFS, UDF, VHD, XAR, Z.
```

# How to deploy?
Deploying is pretty much straight forward and is divided into several steps as follows:
## Installing requirements

- Clone this repo:
```
git clone https://github.com/Iamkillua/myanimemirrorbot
cd mirrorbot
```

- Install requirements
For Debian based distros
```
sudo apt install python3
```
Install Docker by following the [official Docker docs](https://docs.docker.com/engine/install/debian/)

- For Arch and it's derivatives:
```
sudo pacman -S docker python
```
- Install dependencies for running setup scripts:
```
pip3 install -r requirements-cli.txt
```
## Generate Database
<details>
    <summary><b>Click here for more details</b></summary>

**1. Using ElephantSQL**
- Go to https://elephantsql.com/ and create account (skip this if you already have ElephantSQL account)
- Hit **Create New Instance**
- Follow the further instructions in the screen
- Hit **Select Region**
- Hit **Review**
- Hit **Create instance**
- Select your database name
- Copy your database url, and fill to **DATABASE_URL** in config

**2. Using Heroku PostgreSQL**


**NOTE**: If you deploying on Heroku, no need to generate database manually, because it will automatic generate database when first deploying

</details>

## Setting up config file
<details>
    <summary><b>Click here for more details</b></summary>

```
cp config_sample.env config.env
```
- Remove the first line saying:
```
_____REMOVE_THIS_LINE_____=True
```
Fill up rest of the fields. Meaning of each fields are discussed below:
### Required field
- **BOT_TOKEN**: The Telegram bot token that you get from [@BotFather](https://t.me/BotFather)
- **TELEGRAM_API**: This is to authenticate to your Telegram account for downloading Telegram files. You can get this from https://my.telegram.org DO NOT put this in quotes.
- **TELEGRAM_HASH**: This is to authenticate to your Telegram account for downloading Telegram files. You can get this from https://my.telegram.org
- **OWNER_ID**: The Telegram user ID (not username) of the Owner of the bot
- **DATABASE_URL**: Your Database URL. See [Generate Database](https://github.com/breakdowns/slam-mirrorbot/tree/master#generate-database) to generate database. (**NOTE**: If you deploying on Heroku, no need to generate database manually, because it will automatic generate database when first deploying)
- **GDRIVE_FOLDER_ID**: This is the folder ID of the Google Drive Folder to which you want to upload all the mirrors.
- **DOWNLOAD_DIR**: The path to the local folder where the downloads should be downloaded to
- **DOWNLOAD_STATUS_UPDATE_INTERVAL**: A short interval of time in seconds after which the Mirror progress message is updated. (I recommend to keep it `5` seconds at least)  
- **AUTO_DELETE_MESSAGE_DURATION**: Interval of time (in seconds), after which the bot deletes it's message (and command message) which is expected to be viewed instantly. (**Note**: Set to `-1` to never automatically delete messages)
### Optional field
- **AUTHORIZED_CHATS**: Fill user_id and chat_id of you want to authorize.
- **IS_TEAM_DRIVE**: (Optional field) Set to `True` if `GDRIVE_FOLDER_ID` is from a Team Drive else `False` or Leave it empty.
- **USE_SERVICE_ACCOUNTS**: (Optional field) (Leave empty if unsure) Whether to use Service Accounts or not. For this to work see [Using service accounts](https://github.com/breakdowns/slam-mirrorbot#generate-service-accounts-what-is-service-account) section below.
- **INDEX_URL**: (Optional field) Refer to https://github.com/maple3142/GDIndex/ The URL should not have any trailing '/'
- **MEGA_API_KEY**: Mega.nz api key to mirror mega.nz links. Get it from [Mega SDK Page](https://mega.nz/sdk)
- **MEGA_EMAIL_ID**: Your email id you used to sign up on mega.nz for using premium accounts (Leave th)
- **MEGA_PASSWORD**: Your password for your mega.nz account
- **BLOCK_MEGA_FOLDER**: (Optional field) If you want to remove mega.nz folder support, set it to `True`.
- **BLOCK_MEGA_LINKS**: (Optional field) If you want to remove mega.nz mirror support, set it to `True`.
- **STOP_DUPLICATE_MIRROR**: (Optional field) (Leave empty if unsure) if this field is set to `True`, bot will check file in drive, if it is present in Drive, downloading will be stopped. (**Note**: File will be checked using filename, not using filehash, so this feature is not perfect yet)
- **ENABLE_FILESIZE_LIMIT**: Set it to `True` if you want to use `MAX_TORRENT_SIZE`.
- **MAX_TORRENT_SIZE**: To limit the Torrent mirror size, Fill The amount you want to limit, examples: if you fill `15` it will limit `15gb`.
- **IMAGE_URL**: (Optional field) Show Image/Logo in /start message. Fill value of image your link image, use telegra.ph or any direct link image.
- **UPTOBOX_TOKEN**: Uptobox token to mirror uptobox links. Get it from [Uptobox Premium Account](https://uptobox.com/my_account).
- **HEROKU_API_KEY**: (Only if you deploying on Heroku) Your Heroku API key, get it from https://dashboard.heroku.com/account.
- **HEROKU_APP_NAME**: (Only if you deploying on Heroku) Your Heroku app name.
- **SHORTENER_API**: Fill your Shortener api key if you are using Shortener.
- **SHORTENER**: (Optional field) if you want to use Shortener in Gdrive and index link, fill Shortener url here. Examples:
```
exe.io, gplinks.in, shrinkme.io, urlshortx.com, shortzon.com
```
Above are the supported url Shorteners. Except these only some url Shorteners are supported.

**Note**: You can limit maximum concurrent downloads by changing the value of **MAX_CONCURRENT_DOWNLOADS** in aria.sh. By default, it's set to `7`.

</details>

## Getting Google OAuth API credential file

- Visit the [Google Cloud Console](https://console.developers.google.com/apis/credentials)
- Go to the OAuth Consent tab, fill it, and save.
- Go to the Credentials tab and click Create Credentials -> OAuth Client ID
- Choose Desktop and Create.
- Use the download button to download your credentials.
- Move that file to the root of mirrorbot, and rename it to **credentials.json**
- Visit [Google API page](https://console.developers.google.com/apis/library)
- Search for Drive and enable it if it is disabled
- Finally, run the script to generate **token.pickle** file for Google Drive:
```
pip install google-api-python-client google-auth-httplib2 google-auth-oauthlib
python3 generate_drive_token.py
```

## Deploying

- Start Docker daemon (skip if already running):
```
sudo dockerd
```
- Build Docker image:
```
sudo docker build . -t mirrorbot
```
- Run the image:
```
sudo docker run mirrorbot
```

## Deploying on Heroku

For Technical Reasons, Deploying to heroku is disabled for now. 
It Can still be deployed but it is not recommended and may get your account banned!

# Using Service Accounts for uploading to avoid user rate limit
For Service Account to work, you must set **USE_SERVICE_ACCOUNTS=**"True" in config file or environment variables, 
Many thanks to [AutoRClone](https://github.com/xyou365/AutoRclone) for the scripts.
**NOTE**: Using Service Accounts is only recommended while uploading to a Team Drive.

## Generate Service Accounts. [What is Service Account](https://cloud.google.com/iam/docs/service-accounts)

Let us create only the Service Accounts that we need. 
**Warning**: abuse of this feature is not the aim of this project and we do **NOT** recommend that you make a lot of projects, just one project and 100 SAs allow you plenty of use, its also possible that over abuse might get your projects banned by Google. 

```
Note: 1 Service Account can copy around 750gb a day, 1 project can make 100 Service Accounts so that's 75tb a day, for most users this should easily suffice. 
```

`python3 gen_sa_accounts.py --quick-setup 1 --new-only`

A folder named accounts will be created which will contain keys for the Service Accounts

**NOTE**: If you have created SAs in past from this script, you can also just re download the keys by running:
```
python3 gen_sa_accounts.py --download-keys project_id
```

## Add all the Service Accounts to the Team Drive
- Run:
```
python3 add_to_team_drive.py -d SharedTeamDriveSrcID
```

# Youtube-dl authentication using .netrc file
For using your premium accounts in Youtube-dl, edit the netrc file according to following format:
```
machine host login username password my_youtube_password
```
Where host is the name of extractor (eg. Youtube, Twitch). Multiple accounts of different hosts can be added each separated by a new line.

# Credits

Thanks to:
- [breakdowns](https://github.com/breakdowns) Based On His Work
- [out386](https://github.com/out386) heavily inspired from telegram bot which is written in JS
- [Izzy12](https://github.com/lzzy12/) for original repo
- [Dank-del](https://github.com/Dank-del/) for base repo
- [magneto261290](https://github.com/magneto261290/) for some features
- [SVR666](https://github.com/SVR666/) for some features & fixes

And many more people who aren't mentioned here, but may be found in [Contributors](https://github.com/breakdowns/slam-mirrorbot/graphs/contributors).
