# Project Impact: Monolithic to Microservices Migration

## Challenges Before I Joined
- The application followed a monolithic architecture, with all business functionalities packaged into a single Docker image.
- Even a small payment code change required rebuilding the entire application.
- Deployments took longer and increased the risk of impacting unrelated functionalities.
- Independent scaling and service-level releases were difficult.

## My Contribution & Impact
- Identified build and deployment bottlenecks and proposed migrating to a microservices architecture.
- Introduced the approach of independently building and deploying services using Docker, Kubernetes, and service-specific CI/CD workflows.
- Enabled payment-only changes to trigger the payment pipeline without rebuilding or redeploying unrelated services.
- Improved deployment flexibility, service-level scalability, and fault isolation.

## Interview Summary

"In my project, I identified that our monolithic architecture was increasing build times and deployment risks. Even a small payment change required rebuilding and redeploying the entire application. I proposed moving toward microservices, with separate Docker images, CI/CD workflows, and Kubernetes deployments for individual services. This approach enabled targeted deployments, reduced unnecessary build overhead, and improved scalability and fault isolation."
