# Databox Store Mock

A fake data store container to test authentication with. This code is not meant to be run on its own except for debug purposes. The live version is automatically pulled from https://amar.io:5000 as "databox-store-mock" and launched by [container manager](https://github.com/me-box/databox-container-manager) test scripts.

For debug purposes:

## Installation
	git clone https://github.com/me-box/databox-store-mock.git
	cd databox-store-mock
	npm install --production

## Usage
	npm start

Default port is 8080, but can be overridden using the PORT environment variable, i.e.:

	PORT=8081 npm start

Then interface with http://localhost:8080/.

Environment variables that are normally passed by the container manager also need to be set, and DNS resolution (e.g. arbiter -> localhost for debug) needs to be done externally when not running in a container.

## API Endpoints

### /card/:property

#### Description

Method: POST

Updates the record of containers and the extent of their corresponding permissions (default none) maintained by the arbiter.

##### URL Parameters

###### property

Type: string

A property (of depth 1) of a random card object structured as follows:

  - name
  - username
  - email
  - address
    - streetA
    - streetB
    - streetC
    - streetD
    - city
    - state
    - country
    - zipcode
    - geo
      - lat
      - lng
  - phone
  - website
  - company
    - name
    - catchPhrase
    - bs
  - posts[3]
    - words
    - sentence
    - sentences
    - paragraph
  - accountHistory[3]
    - amount
    - date
    - business
    - name
    - type
    - account

Note that while a the mock store makes all this data available, the extent to which a property can be accessed depends on the path caveat encoded in the macaroon used for authentication. E.g. one app may only be allowed to access the path `/card/accountHistory` while another may have access to `/card/name` and `/card/address`.

##### Body Parameters

###### macaroon

Type: string

An arbiter-minted macaroon with the [standard caveats](https://github.com/me-box/databox-arbiter.git).

##### Response

###### Success

  - JSON data structured as specified above ([example](http://faker.hook.io?property=helpers.createCard))

###### Error

  - 400: Missing macaroon
  - 403: Invalid macaroon

### /write

#### Description

Method: POST

Writes posted data into a black hole.

##### Body Parameters

###### macaroon

Type: string

An arbiter-minted macaroon with the [standard caveats](https://github.com/me-box/databox-arbiter#macaroons).

###### data

Type: string

The fake data to be written to the store.

##### Response

###### Success

  - The written data echoed back

###### Error

  - 400: Missing data
  - 400: Missing macaroon
  - 403: Invalid macaroon
