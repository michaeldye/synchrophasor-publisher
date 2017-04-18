# synchrophasor-publisher

A client of a local `pmu-emu` and a cloud-hosted `synchrophasor-dpe`. This service makes outbound connections to the emulator's gRPC server and DPE's gRPC server, brokering data shipment to the latter. This service will cache and retry sending data that haven't successfully been published to the DPE.

Example invocation:

    HZN_HA_PARTNERS="frege,kripke" HZN_LAT="88.2" HZN_LON="-103.1" DEVICE_ID="heidegger" HZN_AGREEMENTID="34636asdf851751157asvz7xx89cv12571923516" PMU_SERVER_ADDRESS=localhost:8008 DPE_SERVER_ADDRESS=localhost:9009 ./synchrophasor-publisher -v 3 -logtostderr

## Related Projects

 * `synchrophasor-proto` (https://github.com/michaeldye/synchrophasor-proto): The protocol specifications for all synchrophasor data projects
 * `synchrophasor-dpe` (https://github.com/michaeldye/synchrophasor-dpe): A DPE data ingest server that is connected-to by `synchrophasor-publisher` clients
 * `pmu-emu` (https://github.com/michaeldye/pmu-emu): A Power Management Unit (PMU) Emulator

## Development

### Preconditions

 * Install `make`
 * Install Golang v.1.7.x or newer, set up an appropriate `$GOPATH`, etc. (cf. https://golang.org/doc/install)
 * Docker version 17.04.0-ce or newer

### Publishing a Docker Container

This project include the make target `publish` that is intended to be executed after a PR has been merged. (Note: this scheme does not have a notion of producing staged development or integration builds, only publishing production stuff. There might be some utility in later producing a `publish-integration` target that is stamped appropriately).

  - Check for an uncommitted files, failing if any exist
  - Clean the project (`make clean`)
  - Build the project (`make all`)
  - Execute all tests (`make test test-integration`)
  - Build a docker container and push it to the repository (`make docker-push`)
  - If the above are successful, tag the `canonical` git repository with the current value in `VERSION`

## Building

### Considerations

This project requires that you build it from the proper place in your `$GOPATH`. Also note that it will automatically install `govendor` in your `$GOPATH` when executing `make deps`.

### Steps

    make
