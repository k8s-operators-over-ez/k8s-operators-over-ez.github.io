## Story - BDD/Gherkin

**TITLE**: Overeasy Operator Requirements

- **DESCRIPTION**
    - **AS A**: Developer
    - **I WANT**: An Operator with a single busybox pod that logs a user specified message and shuts down after a user specified amount of time. If a duration or message are not specified, then both will be supplied by a REST API call. 
    - **SO THAT**: I can demonstrate the encapsulation of operational knowlege, leveraging the Operator Design Pattern.  

- **SCENARIO 1**: Shutdown the busybox pod after a user specified amount of time in seconds
  - **GIVEN**: An Operator instance
  - **WHEN**: the specification `timeout` is set to a numeric value in seconds
  - **THEN**: the busy box pod will remain available for the specified `timeout` duration in seconds,

- **SCENARIO 2**: Log a user specified message before shutting down the busybox pod
  - **GIVEN**: An Operator instance
  - **WHEN**: the specification `message` is set to a string value
  - **THEN**: the busy box pod will log the message, from the `message` specification after the `timeout` duration has expired. 

- **SCENARIO 3**: Retrieve the `timeout` and `message` from a given REST API if one and/or the other is not supplied. 
  - **GIVEN**: An Operator instance
  - **WHEN**: the specification `message` OR `timeout` is NOT set
  - **THEN**: the busy box pod will supply these values from the following REST API: `GET http://my-json-server.typicode.com/keunlee/test-rest-repo/golang-lab00-response`

- **SCENARIO 4**: Update status `expired` and `logged` when the busybox pod has expired
  - **GIVEN**: An Operator instance
  - **WHEN**: the busy box pod's duration has expired
  - **AND**: the busy box pod has logged a message
  - **THEN**: set the operators `expired` status to `true`
  - **AND**: set the operators `logged` status to `true`

### Acceptance Criteria Notes

- The CRD must have a `timeout` specification attribute
- The Operator instance must shut down after the duration of `timeout` in seconds, has been reached
- The CRD must have a `message` specification attribute
- The Operator instance must log the message `message` before the container has stopped
- The CRD must have a `expired` status attribute
- The Operator must set the status of the busy box pod upon expiration, `expired`
- The CRD must have a `logged` status attribute
- The Operator must set the status of the busy box pod when logging a message, `logged`
- The Operator instance must retrieve a `message` and `timeout` value from a REST API call (`GET http://my-json-server.typicode.com/keunlee/test-rest-repo/golang-lab00-response`), if both are not initially supplied on the Operator Instance. 