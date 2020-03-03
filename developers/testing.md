# Testing DART

DART uses [Jest](https://jestjs.io/) for testing. You can run DART's test suite with this command

```
npm test -- --runInBand
```

Note that the `--runInBand` option is required as a number of tests save fixture data to DART's disk-based data store. Because that data is globally accessible, tests must run in sequence to avoid deleting and overwriting data that other tests expect to be in a known state.

## Test Code You Add

If you're adding code to DART, please write tests to ensure your code behaves as expected.
