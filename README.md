[![Betterme](https://telegra.ph/file/7a15e94c5dad454f0d4bc.jpg)](https://youtu.be/s2TktuIA9-s)

# Eunha Gdrive Bot
![GitHub Repo stars](https://img.shields.io/github/stars/vincreator/eunhagdrive?color=blue&style=flat)
![GitHub forks](https://img.shields.io/github/forks/vincreator/eunhagdrive?color=green&style=flat)
![GitHub issues](https://img.shields.io/github/issues/vincreator/eunhagdrive)
![GitHub closed issues](https://img.shields.io/github/issues-closed/vincreator/eunhagdrive)
![GitHub pull requests](https://img.shields.io/github/issues-pr/vincreator/eunhagdrive)
![GitHub closed pull requests](https://img.shields.io/github/issues-pr-closed/vincreator/eunhagdrive)
![GitHub contributors](https://img.shields.io/github/contributors/vincreator/eunhagdrive?style=flat)
![GitHub repo size](https://img.shields.io/github/repo-size/vincreator/eunhagdrive?color=red)
![GitHub commit activity](https://img.shields.io/github/commit-activity/m/vincreator/eunhagdrive)
![GitHub](https://img.shields.io/github/license/vincreator/eunhagdrive)
[![Channel](https://img.shields.io/badge/Channel-blue)](https://t.me/Namexian)

**Eunha Gdrive Bot** is Telegram Bot writen in Python for clone Google Drive into your own Gdrive.

# How to deploy? on vps
Deploying is pretty much straight forward and is divided into several steps as follows:
## Installing requirements

- Clone this repo:
```
git clone https://github.com/vincreator/eunhagdrive/
cd mirrorbot
```

- Install requirements
For Debian based distros
```
sudo apt install python3
pip3 install -r requirements.txt
```

## Deploy on heroku
- Give stars and Fork this repo then upload **token.pickle** to your forks
- Hit the **DEPLOY TO HEROKU** button and follow the further instructions in the screen
- **NOTE**: If you didn't upload **token.pickle**, uploading will not work. How to generate **token.pickle**? [Read here](https://github.com/vincreator/eunhagdrive#getting-google-oauth-api-credential-file)
- Recommended to use 1 App in 1 Heroku accounts

<p><a href="https://heroku.com/deploy"> <img src="https://img.shields.io/badge/Deploy%20To%20Heroku-blueviolet?style=for-the-badge&logo=heroku" width="200""/></a></p>


## Deploy on Heroku with heroku-cli
<details>
    <summary><b>Click here for more details</b></summary>

- Install [Heroku cli](https://devcenter.heroku.com/articles/heroku-cli)
- Login into your heroku account with command:
```
heroku login
```
- Create a new heroku app:
```
heroku create appname
```
- Select This App in your Heroku-cli: 
```
heroku git:remote -a appname
```
- Change Dyno Stack to a Docker Container:
```
heroku stack:set container -a appname
```
- Clone this repo:
```
git clone https://github.com/vincreator/eunhagdrive
ls
cd eunha
```
- get token [Read here](https://github.com/vincreator/eunhagdrive#getting-google-oauth-api-credential-file)
- get sa token (opsional) [Read here](https://github.com/vincreator/eunhagdrive#generate-service-accounts)
- Init the repo clone
```
git init
```
- Add all stuff:
```
git add * -f
git add .gitignore
```
- Commit new changes:
```
git commit -m "EunhaGdrive updates"
```
- Push Code to Heroku:
```
git push heroku master
```
- Restart Worker by these commands,You can Do it manually too in heroku.
- For Turning off the Bot:
```
heroku ps:scale worker=0 -a appname
```
- For Turning on the Bot:
```
heroku ps:scale worker=1 -a appname
```
</details>

## Getting Google OAuth API credential file
<details>
    <summary><b>Click here for more details</b></summary>

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
</details>

## Using Service Accounts for uploading to avoid user rate limit
For Service Account to work, you must set **USE_SERVICE_ACCOUNTS=**"True" in config file or environment variables, 
Many thanks to [AutoRClone](https://github.com/xyou365/AutoRclone) for the scripts.
**NOTE**: Using Service Accounts is only recommended while uploading to a Team Drive.

# Generate Service Accounts
[What is Service Account](https://cloud.google.com/iam/docs/service-accounts)
<details>
    <summary><b>Click here for more details</b></summary>

Let us create only the Service Accounts that we need. 
**Warning**: abuse of this feature is not the aim of this project and we do **NOT** recommend that you make a lot of projects, just one project and 100 SAs allow you plenty of use, its also possible that over abuse might get your projects banned by Google. 

**NOTE:** 1 Service Account can copy around 750gb a day, 1 project can make 100 Service Accounts so that's 75tb a day, for most users this should easily suffice.
```
python3 gen_sa_accounts.py --quick-setup 1 --new-only
```
A folder named accounts will be created which will contain keys for the Service Accounts.

Or you can create Service Accounts to current project, no need to create new one

- List your projects ids
```
python3 gen_sa_accounts.py --list-projects
```
- Enable services automatically by this command
```
python3 gen_sa_accounts.py --enable-services $PROJECTID
```
- Create Sevice Accounts to current project
```
python3 gen_sa_accounts.py --create-sas $PROJECTID
```
- Download Sevice Accounts as accounts folder
```
python3 gen_sa_accounts.py --download-keys $PROJECTID
```
If you want to add Service Accounts to Google Group, follow these steps

- Mount accounts folder
```
cd accounts
```
- Grab emails form all accounts to emails.txt file that would be created in accounts folder
```
grep -oPh '"client_email": "\K[^"]+' *.json > emails.txt
```
- Unmount acounts folder
```
cd -
```
Then add emails from emails.txt to Google Group, after that add Google Group to your Shared Drive and promote it to manager.

**NOTE**: If you have created SAs in past from this script, you can also just re download the keys by running:
```
python3 gen_sa_accounts.py --download-keys project_id
```
</details>

## Add all the Service Accounts to the Team Drive
<details>
    <summary><b>Click here for more details</b></summary>

- Run:
```
python3 add_to_team_drive.py -d SharedTeamDriveSrcID
```
</details>

## Credits

Thanks to:
- [jagrit007](https://github.com/jagrit007) a lot of works
- [Izzy12](https://github.com/lzzy12/) for based code
- [xyou365](https://github.com/xyou365/AutoRclone) AutoRClone
