# Notice: Repository Deprecation
This repository is deprecated and no longer actively maintained. It contains outdated code examples or practices that do not align with current MongoDB best practices. While the repository remains accessible for reference purposes, we strongly discourage its use in production environments.
Users should be aware that this repository will not receive any further updates, bug fixes, or security patches. This code may expose you to security vulnerabilities, compatibility issues with current MongoDB versions, and potential performance problems. Any implementation based on this repository is at the user's own risk.
For up-to-date resources, please refer to the [MongoDB Developer Center](https://mongodb.com/developer).


# REST API with a Cloudflare Worker and a MongoDB Atlas Cluster

This project explains how to build a REST API in a Cloudflare worker using MongoDB Realm Web SDK and a MongoDB Atlas cluster to store the data.

# How it Works

- The MongoDB Atlas cluster stores the data in the `cloudflare.todos` collection.
- A MongoDB Atlas App Services App manages the authentication and the collection access rules.
- A Cloudflare worker uses the Realm Web SDK to authenticate and retrieve the data that is then exposed with a REST API.

# Prerequisites

- MongoDB Cloud account.
- MongoDB Atlas Cluster (M0 is fine).
- MongoDB Atlas App Services Application created & deployed.
  - with Authentication API Keys turned on + an API key created.
  - with a rule on the collection `cloudflare.todos` with a role "owner" with read and write access on all the fields, applied when `{"owner": "%%user.id"}`.
- Cloudflare account (free plan is fine) with a `*.workers.dev` subdomain.

To deploy & test the API we need:
- The Atlas App Services Application ID (top left corner).
- The App authentication API key (in Authentication tab > API Keys).
- The Cloudflare account login/password.
- The Cloudflare account ID (in Workers tab > Overview).
- The Cloudflare `*.workers.dev` subdomain (in Workers tab > Overview).

# Build and Deploy

Clone this repository
```shell
git clone git@github.com:mongodb-developer/cloudflare-worker-rest-api-atlas.git
cd cloudflare-worker-rest-api-atlas
```

Edit the file `wrangler.toml`
- replace `CLOUDFLARE_ACCOUNT_ID` by your real Cloudflare account ID.
- replace `MONGODB_REALM_APPID` by your real MongoDB Atlas App Services App ID.

If you want to use the bash files to test the REST API, edit the file `api_tests/variables.sh`:
- replace `YOUR_SUBDOMAIN` so the final worker URL matches yours.
- replace `YOUR_REALM_AUTH_API_KEY` with your App auth API key.

Run the following commands:

```shell
$ npm i wrangler -g
$ wrangler login
$ wrangler deploy
$ cd api_tests
$ ./post.sh "Write a good README.md for Github"
$ ./post.sh "Commit and push"
$ ./findAll.sh
$ ./findOne.sh <OBJECT_ID> # replace with an _id from the previous command
$ ./patch.sh <OBJECT_ID> true
$ ./findAll.sh # note that done=true now on your todo
$ ./deleteOne.sh <OBJECT_ID>
$ ./findAll.sh # only one left
```

You can also navigate to your MongoDB Atlas Cluster and browse your collection `cloudflare.todos` to confirm the above tests.

# Authors

- [Luke Edwards](https://x.com/lukeed05)
- [Maxime Beugnet](https://x.com/mbeugnet) <maxime@mongodb.com>
