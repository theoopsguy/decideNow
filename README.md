# DecideNow

To spin up the container:
1. Use `docker-compose up` from root directory.

To start the server:
1. `cd server`
2. `yarn`
3. `yarn start:dev`

Project architecture/working brief:
- We have 4 modules: Config Module, JWT Module, Redis Module and Polls Module
- Polls module has a WebSocketGateway to enable realtime updates of the polls data b/w client and server 
- We'll also have REST HTTP API which will be used to make requests to create poll/join poll and when web socket connection fails as a fallback HTTP connection will be used.
