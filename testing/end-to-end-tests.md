# End to End tests

## Running the test

1. Prepare an empty test database and pass connection URL to DB_URL environment variable and make sure that database is running.
2. Install dependencies `npm install`
3. Run the test `npm run test:e2e`

## Tests

- [x] Can't access protected endpoint while unauthenticated
- [x] Registers a user
- [x] Activates a user
- [x] Can login and get a JWT
