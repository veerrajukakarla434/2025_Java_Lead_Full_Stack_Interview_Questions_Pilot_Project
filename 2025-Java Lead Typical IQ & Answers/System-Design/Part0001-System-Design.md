# System Design Interview Questions

![image](https://github.com/user-attachments/assets/f1aebc11-289e-4343-b6cf-a24abc1348e1)

![image](https://github.com/user-attachments/assets/5425063a-f055-4d59-a866-4063f211d795)

![image](https://github.com/user-attachments/assets/bb7ad926-4df6-44c3-8728-80adf9eca5d4)

![image](https://github.com/user-attachments/assets/dc9955ef-1e55-4369-ac3c-265f5898cc03)


#### System Design Life Cycle | SDLC (Design)
*  https://www.geeksforgeeks.org/system-design/system-design-life-cycle-phases-models-and-use-cases/
#### Most Commonly Asked System Design Interview Problems/Questions
* https://www.geeksforgeeks.org/system-design/most-commonly-asked-system-design-interview-problems-questions/



* System Design Master Template -> https://www.designgurus.io/course-play/grokking-the-system-design-interview/doc/system-design-master-template

  
#### 1) Difference between an API gateway and a load balancer
Ans:

An API gateway is focused on routing requests to the appropriate microservice, while a load balancer is focused on distributing requests evenly across a group of backend servers.
Another difference between the two is the type of requests that they typically handle. An API gateway is typically used to handle requests for APIs, which are web-based interfaces that allow applications to interact with each other over the internet. These requests typically have a specific URL that identifies the API that the client is trying to access, and the API gateway routes the request to the appropriate microservice based on this URL. A load balancer, on the other hand, is typically used to handle requests that are sent to a single, well-known IP address, and then routes them to one of many possible backend servers based on factors such as server performance and availability.
