# Release instruction

## FE
Deployment of FE part is configured to work with Capistrano tool. Whole process is separated to two parts. First is building app locally(unfortunately) and second one is just pushing everything to the remote server. 

### Building locally
- Setup `.env.production`/`.env.staging` file
- Run `yarn run build:web`
- Pushing to remote server
- Run `cap env deploy` where `env` might `production`, `staging` or `test`
- When command is successfully executed FE application is already published
- Whole process is already implemented on our Continuous Integration service so it’s kinda automated. Just push code on branch named like environment - `staging` or `production`, CircleCI will prepare everything and wait for “approval”(it’s action specific for CircleCI, should be executed from dashboard.

## BE 
Deployment of BE part is configured to work with Capistrano tool. Everything is automated so one thing which need to be done is run Capistrano command:

`cap env deploy` where `env` might `production`, `staging` or `test`

Deployment process is already implemented on our Continuous Integration service so it’s kinda automated. Just push code on branch named like environment - `staging` or `production`, CircleCI will prepare everything and wait for “approval”(it’s action specific for CircleCI, should be executed from dashboard.

Additional note: when application need to be replicated to next server we need to instal puppeteer for handling PDF generating. Just run `yarn install` and puppeteer will be installed on server. 

## Mobile
We can distinguish four deployment paths. For this path we assume:
- User has dependencies installed through yarn
- User is signed in on expo account, user: developer.monergie
- User has correctly set up:
- /src/mobile/.env.development
- /src/mobile/.env.test
- /src/mobile/.env.production

### Test path
Expo is used as a deployment platform. In root FE folder type `yarn publish:mobile:test` and then the app will be available in Expo app under https://exp.host/@developer.monergie/monergie-dev?release-channel=test 

### Test - simulator path
In root FE folder type `yarn build:test:ios`. The pp will be available both in the link above and as standalone app file available for install on simulators (link will be shown after build ends).

Warning: this key can be required in .env.test for such build:
```
KEYSTORE_ALIAS=
KEYSTORE_PATH=
KEYSTORE_PASSWORD=
KEY_PASSWORD=
P12_PASSWORD=
P12_PATH=
PROVISIONING_PROFILE_PATH=
```

### Stage path
Circle CI tools similarly automate this build as FE. After pushing changes on staging branch you need to open workflow for this commit and approve stage release. After that, it will be available on Expo using link https://expo.io/@developer.monergie/monergie-dev?release-channel=staging-staging-v<number> where <number> is available after CircleCI will finish deploying. 

### Production path
In root FE folder type `yarn build:production:ios` for ios build and `yarn build:production:android` for android. Remember about incrementing build number for android and version name for ios. After the build, there will be a link for downloading the build.
 #### Android path:
Sign in on google play developer console. In app releases, create a new release with downloaded apk file and release new version.

#### Ios path:
Using Transporter app for mac, upload ipa file to the App Store. In https://appstoreconnect.apple.com/ create new version and add parsed build. Then release it.
