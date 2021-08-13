## Applications in SRE Role

The Monitoring, Automation and Eliminating the toil are some of the core pillars of the SRE discipline. As an SRE, you may require spending about 50% of time on automating the repetitive tasks and to eliminate the toil. CI/CD pipelines are one of the crucial tools for the SRE. They help in delivering the quality application with the smaller and regular and more frequent builds. Additionally, the CI/CD metrics such as Deployment time, Success rate, Cycle time and Automated test success rate etc. are the key things to watch to improve the quality of the product thus improving the reliability of the applications.

* [Infrastructure-as-code](https://en.wikipedia.org/wiki/Infrastructure_as_code) is one of the standard practices followed in SRE for automating the repetitive configuration tasks. Every configuration is maintained as code, so it can be deployed using CI/CD pipelines. It is important to deliver the configuration changes to the production environments through CI/CD pipelines to maintain the versioning, consistency of the changes across environments and to avoid manual errors.
* Often, as an SRE, you are required to review the application CI/CD pipelines and recommend additional stages such as static code analysis and the security and privacy checks in the code to improve the security and reliability of the product.

## Conclusion

In this chapter, we have studied the CI/CD pipelines with brief history on the challenges with the traditional build practices. We have also looked at how the CI/CD pipelines augments the SRE discipline. Use of CI/CD pipelines in software development life cycle is a modern approach in the SRE realm that helps achieve greater efficiency.

We have also performed a hands-on lab activity on creating the CI/CD pipeline using Jenkins.

## References

1.	[Continuous Integration(martinfowler.com)](https://martinfowler.com/articles/continuousIntegration.html)
2.	[CI/CD for microservices - Azure Architecture Center | Microsoft Docs](https://docs.microsoft.com/en-us/azure/architecture/microservices/ci-cd)
3.	[SREFoundationBlueprint_2 (devopsinstitute.com)](https://www.devopsinstitute.com/wp-content/uploads/2020/11/SREF-Blueprint.pdf)
4.	[Jenkins User Documentation](https://www.jenkins.io/doc/)