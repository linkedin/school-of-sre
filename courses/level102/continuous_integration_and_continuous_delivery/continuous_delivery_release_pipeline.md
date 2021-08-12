***Continuous Delivery*** means deploying the application builds more frequently in the non-production environments such as [SIT, UAT, INT](https://medium.com/@buttertechn/qa-testing-what-is-dev-sit-uat-prod-ac97965ce4f) and performing the integration tests and the acceptance tests automatically.

In the CD, the tests are performed on the integrated application instead of the single microservice in the cases of microservice based application. The tests must include all the functional tests and the acceptance tests that may contain the UI tests. The build must be immutable in nature, that is the same package must be deployed across all the environments including the Production.

The deployment to the Production is often manual after performing additional acceptance tests such as performance tests etc. So, the fully automated deployment to the Production environments is called the ***Continuous Deployment*** (whereas ***CD – Continuous delivery*** doesn’t automatically deploy to Production). The continuous deployment must have a [feature toggle](https://martinfowler.com/articles/feature-toggles.html) so that a feature can be toggled off without the need for redeploying the code.

Often, the deployment involves more than one production environment, for example in [blue-green environments](https://www.linkedin.com/pulse/using-blue-green-deployments-reduce-downtime-nessan-harpur) the application is first deployed to the blue environment and then to the green environment so that the downtime is not required.


![](./images/CD_Image1.jpg)

*Fig 3: Continuous Delivery Pipeline*