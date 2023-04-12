# Zeebe Process Testing and Deployment
Deployment and testing of zeebe workflows will be a handled in a separate git submodule - [deploy-and-test](https://github.com/respo-financial/zype-processes-test)

## How to test processes and jobs?
Testing the entire process along with jobs can be divided into three different scenarios

- E2E Testing -- This includes writing test cases to check if the entire end-to-end flow, i.e, API call -> process invocation -> run actual jobs -> process completion -> response. The entire cycle is tested with no mocks
- Jobs unit testing -- Write test cases for job workers, while mocking i/o layer (DB calls, 3rd party API calls,etc). The aim of these test cases are to test the core business logic, without worrying about the actual i/o response. Every type of possible request response will be created and test cases will be written against these

- Process testing -- Write test cases to ensure the workflows(BNPM files) work as expected. Each different paths have to be tested against all possible(success and error) cases

## Process Testing Setup

- Clone the [deploy-and-test](https://github.com/respo-financial/zype-processes-test) repo in your local
- Install Intellij (Recommended) or any IDE of your choice
- Open zype-process-test and set JAVA sdk version > 17
- Alternatively if you want to run from terminal, setup JDK version 17 ,then RUN
```sh
cd zype-processes-test && mvn clean install
```
- Go to src.test.java.com.zype.zeebe.deploy
- Add a new file with naming convention '${BNPM_PROCESS_NAME}Test' in camelCase
- Use deployTestBNPM() method 
```sh
    public void deployTestBNPM(ZeebeClient client, String processName) {
        client.newDeployCommand()
                .addResourceFromClasspath(processName)
                .send()
                .join();
    }
```
to deploy the BNPM workflow before testing process instance. Add it to @BeforeEach method 
To read more on how JUNIT 5 lifecycles work - [junit 5 lifecycle](https://howtodoinjava.com/junit5/junit-5-test-lifecycle/)
```sh
	@BeforeEach
	public void deployResources(){
		TestUtils.deployTestBNPM(client,"apiretrysubprocess.bpmn");
	}
```
- Similarly use initProcessInstanceStart() and initProcessInstanceStartWithVariables() from TestUtils.java to create and invoke the process instance
- Annotate test cases with @Test to run test cases
- To actually execute test cases use Intellij to quickly run and debug your test cases


## Test a flow in process
For every file we create, we will assume it to be a test suite, All possible flows have to be written separately as @Test 

> For example in api retries we have 2 flows
>when counter <= allowedRetries
>when counter > allowedRetries
>for both of the workflows test with multiple  variables

Process can be tested in a systematic manner 
- Deploy resource and assert
- Create instance process and assert
- Activate Jobs, then fetch an activated job and assert
- Assert if processInstance isActive
- Continue to activate and assert Jobs till end
- Assert if processInstance isCompleted


## Useful Links
- [spring-zeebe](https://github.com/camunda-community-hub/spring-zeebe) - Our deploy and process project is using this for deployments and integrating with test cases
- [junit5](https://www.baeldung.com/junit-5) - Our test case writing framework
- [zeebe-process-test](https://github.com/camunda/zeebe-process-test)- Dependecy to run tests for processes
- [camunda-official-guide](https://camunda.com/blog/2022/01/testing-processes-for-camunda-cloud-and-zeebe/) - Cammunda guide on how to write test cases for processes
- [maven-tutorial](https://www.baeldung.com/maven) - Build tool that we are using
- [twitter-example](https://github.com/camunda-community-hub/camunda-8-examples/blob/main/twitter-review-java-springboot/src/test/java/org/camunda/community/examples/twitter/TestTwitterProcess.java) - Test case example
- [another-example](https://github.com/camunda/zeebe-process-test/tree/main/examples) - Another example


## TO DO
- Add test case coverage for processes (Should be 100% always)
- Add more details to this doc




