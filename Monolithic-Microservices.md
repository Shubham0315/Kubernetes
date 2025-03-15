# Monolithic Architecture

- Here, application is built as single, unified unit. All the components such as UI, DB and external integrations are tightly coupled and packaged together
- Characteristics:
  - **Single Codebas**e - All components are part of one large database which is compiled and deployed as one unit
  - **Tightly Coupled** - All parts of application depend on each other. Change in one part can affect whole system'
  - **Shared Memory** - As all components run together, they typically share memory and resources
  - **Single Deployment** - Entire application is deployed together and scaling application means scaling entire system. So scaling being harder
 
- **Pros** - Simplicity, Easier communication, Less complex to manage
- **Cons** - Limited scalability, Hard to maintain changes over time, Deployment risks as change or bug in system require redeployment, Hard to adapt new technologies due to tight coupling

- It is ideal for small teams or simple applications that dont need to scale quicky or manage a lot of complexity
- Suitable for projects where rapid initial development is priority


----------------------------------------------------------------------------------------------------

# Microservices Architecture

- The application is broken down into set of small, independent services where each service is responsible for specific function and communicates with other services over a network
- Characteristics;
  - **Independent Services** - Each microservice focuses on specific business capability and is developed, deployed and scaled independently
  - **Loose Coupling** - Microservices are loosely coupled and communicate through lightweight protocols like HTTP or messaging queues
  - **Distributed Deployment** - Each MS is deployed independently which allows for flexibility in updates and scaling
  - **Technological Diversity** - Diff services can be written in different languages or use diff DB
  - **Independent scaling** - We can scale individual services
 
- **Pros** - Scalability, Flexibility, Faster deployment, easier to maintain small codebases
- **Cons** - Complexity to manage many services, Latency and additinal overhead , difficult to maintain data consistency, Require skilled teams due to complexity

- It is ideal for large, complex applications which needs to scale eficiently and when you need to isolate failure points or allow for independent updates
- Suitable for teams that can manage increases complexity of managing multiple services and dependencies
