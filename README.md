# DecideNow

## Project Setup
To spin up the container:
1. Use `docker-compose up` from root directory.

To start the server:
1. `cd server`
2. `yarn`
3. `yarn start:dev`

## Project architecture/working brief:
- We have 4 modules: Config Module, JWT Module, Redis Module and Polls Module
- Polls module has a WebSocketGateway to enable realtime updates of the polls data b/w client and server 
- We'll also have REST HTTP API which will be used to make requests to create poll/join poll and when web socket connection fails as a fallback HTTP connection will be used.

### Web Sockets authorization
To authorize the socket connection requests and to ensure that the event emitted is only visible to the participants of a poll, we have multiple approaches:
1. In the socketIOAdapter, add an allowRequest method which will do the validations. 
The problems with this approach are:

- Limited Context: Since the allowRequest method only receives a raw web sockets request coming in as HTTP before the WebSocket connection is established, it lacks context about the user or the specific WebSocket connection being requested. This limitation makes it challenging to perform user-specific authorization checks or enforce fine-grained access control.

- Inability to Access Socket or Client: As mentioned, the allowRequest method doesn't have access to the WebSocket connection or the client object, which are typically necessary for performing authorization and filtering events based on participants. Without access to these objects, it's challenging to enforce rules specific to individual connections or clients.

2. Using a middleware - Utilizing middleware functions provided by Socket.IO to handle authorization and validation logic. Middleware can intercept incoming WebSocket events and perform checks before they reach the event handlers, providing more flexibility and context. Let's use this approach.