## Execution Strategy (Optional Reading)

In a nutshell, we want our operator to start up a pod, running a busybox image for a specific duration and logging a user specific message, and then setting our Operator's status. 

We'll want our Operator to provision our busybox pod with the necessary attribute specifications, eventually. 

Our strategy to reach the end state is detailed as followed: 

![](/assets/execution-strategy.png)

- **I - Prototyping** - Create a YAML specification for a pod which runs for a specified amount of time and logs a specific message. Do this to validate our design. We'll eventually want our Operator controller implementation to dynamically set the pods timeout duration and log message. For now, we will validate our prototype. 

- **II - Operator Scaffolding** - Scaffold a Golang Operator to give us an initial template for our CRD and Resource Controller

- **III - CR Definition Implementation** - Add properties for your Custom Resources Specifications and Status attributes. 

- **IV - TDD Setup** - Create a Unit Test file for our Custom Resource Controller to validate our requirements leveraging TDD (Test Driven Design). We will validate the tests as we implement our controller. 

- **V - CR Controller Implementation**- Implement our Resource Controller logic to help fulfill the Story and Acceptance Criteria.

- **VI - Test Validation** - Validate our Unit Tests. Sanity check our Operator so that it is indeed operating as intended. 

- **VII - Deployment** - Deploy the Operator to your Kubernetes cluster