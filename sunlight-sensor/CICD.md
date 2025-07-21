# Continuous Integration and Continuous Delivery (CICD)

This project uses GitHub actions to automatically deploy the React-based webapp to Google Cloud Platform.  Because this is a portfolio project, and there are not customers dependent on it 24/7, it is deployed directly to the production environment.

There are some unit tests throughout the project, but code coverage is currently a work in progress.

If this were a production project, ideally it would minimally:

1. Pass automated unit tests at commit time as part of the build

2. Pass some code review

3. Be deployed to a shared QA environment for manual and automated testing 

4. Pass tests including
   
   1. Functional integration tests
   
   2. Checks for insecure dependencies
   
   3. Licensing checks

5. Be approved for deployment to the production environment.
