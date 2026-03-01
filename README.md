
# Lab 3: Deploying the Algonquin Pet Store on Azure

**Student:Harshdeep


##  Demo Video (Link)


---

##  Project Links

This project is composed of three microservices and a RabbitMQ broker:

| Component | Repository Link | Deployed URL |
| --- | --- | --- |
| **Store Front** (Vue.js) | [Link to Store Front](https://github.com/harshdeep1230/store-front-lab2) | `https:/` |
| **Order Service** (Node.js) | [Link to Order Service](https://github.com/harshdeep1230/order-service-lab2) | `https://order-service-app.azurewebsites.net` |
| **Product Service** (Rewritten) | [Link to Product Service](https://github.com/harshdeep1230/product-service-lab2) | `https://product-service-app.azurewebsites.net` |
| **RabbitMQ Broker** | N/A (Hosted on Azure VM) | `http://52.139.34.219:15672` |

---

## Reflection Questions

### 1. What challenges did you encounter when configuring environment variables in the GitHub Actions workflow?

The main issue was to comprehend that the environment variables in the Vue.js environment need to be pre-prefixed with VUE_APP_ in order to be detected during the building process. In contrast to the backend services, which read variables during runtime, these values are built into the static files when building with GitHub Actions. It was necessary to make sure that the YAML syntax of the files in the .github/workflows/ was correctly indented and contained the URLs that contain the https:// protocol in order to communicate with the service successfully.


### 2. How does deploying microservices on Azure Web App Service differ from running them locally?

The deployment on the Azure Web App Service will place the burden of managing the infrastructure in the hands of the cloud provider (PaaS). I handle the runtime, port and OS dependencies locally. The platform in Azure does scaling, security patching and offers managed CI/CD pipeline through GitHub Actions. Moreover, the networking modification is done to switch to public DNS endpoints instead of localhost, which needs more rigid firewall (NSG) and CORS settings.


### 3. Why is it important to use environment variables for configurations in a cloud environment?

The use of environment variables is very essential in adhering to the 12-Factor App methodology Factor III: Config. They enable the identical code to be used across (Dev, Test, Prod) environments without any adjustment. They do not place sensitive credentials in the source code, such as the RabbitMQ connection string, in a cloud environment to avoid security leaks and instead leave the values injected at runtime by the Azure platform.

---

## SEtup Notes & Lessons Learned

* **RabbitMQ Security:** I came to know that the user idle is a localhost-only user. I needed to add a custom user to the administration group in order to enable the order-service (running in the Azure App Service) to communicate with the RabbitMQ VM.
* 
* **Azure Networking:** Opening port `15672` for the UI and `5672` for AMQP traffic in the Network Security Group is important step for connectivity.
* **Service Rewrite:** The need to rewrite the Product Service in Rust to a natively supported program (such as Python/Node) demonstrated the significance of compatibility on a platform in the cloud-native design.
