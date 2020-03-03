# REST-API-Node-Boilerplate

A simple and customizable RESTful API boilerplate written in [Node.js](https://nodejs.org/en/) using [Express](https://expressjs.com/). The boilerplate is backed with a [MySQL](https://www.mysql.com/) and a [Mongodb](https://www.mongodb.com/) to store large files.



# Features

- ES6 support using [Babel](https://babeljs.io/)
- Auto server restart thanks to [nodemon](https://github.com/remy/nodemon)
- Async/Await syntax
- Authentication using [jsonwebtoken](https://jwt.io/)
- Body parsing
- Cross-Origin Resource Sharing enabled
- Consistent coding styles
- Docker + data persistence
- Express web framework
- Linting with [eslint](https://eslint.org/)
- Logging using [winston](https://github.com/winstonjs/winston)
- MySQL table hydratation
- Support http/https
- Uses [Yarn](https://yarnpkg.com/en/) over npm
- Test using [AVA](https://github.com/avajs/ava)



# Requirements

- [Docker](https://www.docker.com)
- [Node.js 12+](https://nodejs.org/en/)
- [Yarn](https://yarnpkg.com/)


For MacOS, add the following paths to your docker engine:

- `/var/lib/mysql`
- `/var/lib/mongodb/data/db`

This can be done through **Docker -> Preferences -> File sharing**


# Contribution

Each Contribution is welcomed and encouraged. I do not claim to cover each use cases nor completely master the Node.js. If you encounter a non sense or any trouble, you can open an issue and I will be happy to discuss about it :smile:



# Tests

Run the following commands to run the unit/integration tests:

 ```bash
$ yarn test
 ```



# Usage

Run the following commands to use your boilerplate:

 ```bash
# Start the stack
$ docker-compose up --build --detach

#
# Assuming your are using the default config
#

# Ping the service
$ curl --request GET http://localhost:3001/api.boilerplate

# Register a new account
$ curl -H "Content-Type: application/json" --request POST \
  -d '{"username":"foo", "email":"foo@email.com", "password":"bar"}' \
  http://localhost:3001/api.boilerplate/register

# Authorize your account and retrieve your authentication token
$ curl -H "Content-Type: application/json" --request POST \
  -d '{"username":"foo", "password":"bar"}' \
  http://localhost:3001/api.boilerplate/authorize

# Test an auth required request
$ curl -H "Authorization: INSERT_YOUR_TOKEN" --request GET \
  http://localhost:3001/api.boilerplate/hello

# Upload a picture (okay this is not a picture ... :p)
curl -H "Authorization: INSERT_YOUR_TOKEN" -F file=@README.md \
  -X POST http://localhost:3001/api.boilerplate/picture

# Get your pictures
curl -H "Authorization: INSERT_YOUR_TOKEN" --request GET \
  -X POST http://localhost:3001/api.boilerplate/pictures

# Get a specific picture
curl -H "Authorization: INSERT_YOUR_TOKEN" --request GET \
  -X POST http://localhost:3001/api.boilerplate/picture/PICTURE_ID

# Delete a specific picture
curl -H "Authorization: INSERT_YOUR_TOKEN" --request DELETE \
  -X POST http://localhost:3001/api.boilerplate/picture/PICTURE_ID

# Unregister your account
$ curl -H "Content-Type: application/json" --request DELETE \
  -d '{"username":"foo", "password":"bar"}' \
  http://localhost:3001/api.boilerplate/unregister
 ```



# Customization

## Config
Check the [config](https://github.com/TommyStarK/REST-API-Node-Boilerplate/blob/master/server/config/index.js) file to customize your boilerplate as you wish.

  ```js
  {
    app: {
      name: 'Experimental REST API boilerplate',
      url: 'api.boilerplate',
      http: { port: 3001 },
      https: {
        port: 8443,
        tls: { certificate: 'server.crt', key: 'key.pem', path: 'tls/' },
      },
      secret: '1S3cR€T!',
      expiresIn: '24h',
      production: false,
    },
    mongo: {
      port: '27017',
      uri: process.env.MONGO_URI || 'localhost',
      database: 'experimental_rest_api_boilerplate_mongodb',
    },
    mysql: {
      host: process.env.MYSQL_URL || '127.0.0.1',
      user: 'root',
      password: 'root',
      database: 'experimental_rest_api_boilerplate_mysql',
    },
  }
  ```

By default, the config looks like this.


## HTTPS

To enable https, you must add your tls certificate and key to `tls/` before running your boilerplate:

To generate self-signed certificate, run the following commands:
```bash
$ openssl req -newkey rsa:2048 -new -nodes \
  -keyout tls/key.pem -out tls/csr.pem

$ openssl x509 -req -days 365 -in tls/csr.pem \
  -signkey tls/key.pem -out tls/server.crt
```
