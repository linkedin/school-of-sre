## The Evolution of the CI/CD

Traditional development approaches have been around for a very long time. The [waterfall model](https://www.linkedin.com/pulse/waterfall-model-shobika-ramasubbarayalu) has been widely used in both large and small projects and has been successful. Despite the success, it has a lot of drawbacks like longer cycle times or delivery.

While multiple team members are working on the project, the code changes get accumulated and never integrated until the planned build date. The build usually happens on agreed cycles that range from a month to a quarter. This results in several integration issues and build failures as the developers were working on their features in silos.

It was a nightmare situation for the operations teams/for anyone to deploy the new builds/releases to the production environment because of lack of proper documentation on every change and the configuration requirements. So, to deploy successfully, often it required hot fixes and immediate patches.

Another big challenge was collaboration. It is rare that the developer meets the operation engineers and does not have a full understanding of the production environment. All these challenges have given rise to longer cycle times for the delivery of the code changes.

[Agile](https://www.linkedin.com/pulse/list-popular-agile-methodologies-used-organizations) methodology prescribes the delivery of incremental delivery of features in multiple iterations. So, the developers commit their code changes in smaller increments and roll out more frequently. Every code commit triggers a new build, and the integration issues are identified much early.  This has improved the build process and thereby reduced the cycle time. This process is known as *continuous integration or CI*.

The big barrier between the developers and the operation teams has been shrunken with the emergence of the trend where organizations are adapting to the DevOps and SRE disciplines. The collaboration between the developers and the operation teams is improved. Moreover, the use of the same tools and processes by both the teams has improved coordination and avoided conflicting understanding of the process. One of the main drivers in this regard is the *continuous delivery (CD)* process that ensures the incremental deployment of smaller changes. There are multiple pre-production environments also called the staging environments before deploying to production environments.

## CI/CD and DevOps

The term **DevOps** represents the combination of Development (Dev) and Operations (Ops) teams. That is bringing developers and operations teams together for more collaboration. The development team often wants to introduce more features and more changes while the operation teams are more focused on the stability of the application in production. A change is always taken as a threat by the operations team as it can shake the stability of the environment. DevOps is termed as a culture that introduces the processes to reduce the barriers between developers and operations.

The collaboration between Dev and Ops allows better follow-up of end-to-end production deployments and more frequent deployments. So, thus CI/CD is a key element in the DevOps processes.
