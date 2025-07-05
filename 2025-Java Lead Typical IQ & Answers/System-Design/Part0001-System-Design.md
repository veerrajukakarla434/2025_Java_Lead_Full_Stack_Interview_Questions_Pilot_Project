# System Design Interview Questions

* https://www.geeksforgeeks.org/system-design/most-commonly-asked-system-design-interview-problems-questions/



* System Design Master Template -> https://www.designgurus.io/course-play/grokking-the-system-design-interview/doc/system-design-master-template

  
#### 1) Difference between an API gateway and a load balancer
Ans:

An API gateway is focused on routing requests to the appropriate microservice, while a load balancer is focused on distributing requests evenly across a group of backend servers.
Another difference between the two is the type of requests that they typically handle. An API gateway is typically used to handle requests for APIs, which are web-based interfaces that allow applications to interact with each other over the internet. These requests typically have a specific URL that identifies the API that the client is trying to access, and the API gateway routes the request to the appropriate microservice based on this URL. A load balancer, on the other hand, is typically used to handle requests that are sent to a single, well-known IP address, and then routes them to one of many possible backend servers based on factors such as server performance and availability.
