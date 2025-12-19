# AWS Lambda – Interview Questions & Answers

---

## 1. What is AWS Lambda?

**Answer:**  
AWS Lambda is a **serverless compute service** that runs code in response to events without provisioning or managing servers. You pay only for execution time and number of requests.

---

## 2. Why is Lambda called serverless?

**Answer:**  
Servers still exist, but AWS fully manages:

- Server provisioning
- OS
- Scaling
- Availability
- Patching  
  Developers focus only on writing code.

---

## 3. What are common Lambda use cases?

**Answer:**

- API backends (with API Gateway)
- Event processing (S3, DynamoDB)
- Data transformation
- Image resizing
- Background jobs
- Scheduled tasks (cron jobs)

---

## 4. What are Lambda triggers?

**Answer:**  
Triggers are AWS services that invoke Lambda automatically, such as:

- API Gateway
- S3
- DynamoDB Streams
- SQS
- EventBridge
- CloudWatch Logs
- Step Functions

---

## 5. What is a Lambda function?

**Answer:**  
A Lambda function is a **unit of code** that runs in response to an event. It includes:

- Runtime (Node.js, Python, Java, etc.)
- Handler function
- IAM execution role

---

## 6. What runtimes are supported in Lambda?

**Answer:**

- Node.js
- Python
- Java
- Go
- .NET
- Ruby  
  You can also use **custom runtimes**.

---

## 7. What is a Lambda handler?

**Answer:**  
The handler is the entry point function that AWS Lambda executes when the function is invoked.

---

## 8. What is the maximum execution time of Lambda?

**Answer:**  
**15 minutes (900 seconds)** per invocation.

---

## 9. What is Lambda cold start?

**Answer:**  
Cold start occurs when Lambda initializes a new execution environment, causing additional latency. It usually happens when:

- Function hasn’t been invoked recently
- Function is inside a VPC

---

## 10. How to reduce cold start?

**Answer:**

- Use Provisioned Concurrency
- Reduce package size
- Avoid VPC unless required
- Use lightweight runtimes (Node.js, Python)

---

## 11. What is Provisioned Concurrency?

**Answer:**  
It keeps Lambda execution environments **pre-warmed**, eliminating cold starts at additional cost.

---

## 12. What is Lambda concurrency?

**Answer:**  
Concurrency is the number of Lambda instances running simultaneously.

- Default account limit: **1000 concurrent executions**
- Can be increased via AWS support

---

## 13. What happens if concurrency limit is exceeded?

**Answer:**  
Requests are throttled, and Lambda returns **429 errors**.

---

## 14. What is Lambda memory allocation?

**Answer:**  
Memory can be configured from **128 MB to 10 GB**.  
CPU and network scale **proportionally** with memory.

---

## 15. Is Lambda stateless?

**Answer:**  
Yes. Each invocation is independent.  
Persistent data must be stored in external services like:

- DynamoDB
- S3
- RDS
- ElastiCache

---

## 16. Can Lambda access resources in a VPC?

**Answer:**  
Yes. Lambda can be attached to a VPC to access:

- RDS
- ElastiCache
- Private EC2 services

---

## 17. What are downsides of running Lambda in a VPC?

**Answer:**

- Increased cold start latency
- Requires NAT Gateway or VPC endpoints for internet access
- ENI management overhead

---

## 18. How does Lambda connect to the internet from a VPC?

**Answer:**  
Using:

- NAT Gateway (for public internet)
- VPC Endpoints (for AWS services)

---

## 19. What is Lambda pricing based on?

**Answer:**

- Number of invocations
- Execution duration (ms)
- Memory allocated
- Provisioned concurrency (if used)

---

## 20. What is Lambda timeout?

**Answer:**  
Maximum allowed execution time for a function. Default is **3 seconds**, max is **15 minutes**.

---

## 21. What is Lambda environment variable?

**Answer:**  
Key-value pairs used to configure functions without changing code.  
Often used for:

- DB endpoints
- Feature flags
- Configuration values

---

## 22. How do you secure Lambda?

**Answer:**

- IAM execution roles
- Least privilege policies
- Resource-based policies
- No inbound ports
- Optional VPC isolation

---

## 23. What is Lambda IAM execution role?

**Answer:**  
An IAM role that grants Lambda permission to access other AWS services like S3, DynamoDB, or CloudWatch.

---

## 24. Can Lambda be used for long-running tasks?

**Answer:**  
No. Lambda is not suitable for tasks exceeding **15 minutes** or requiring persistent state.

---

## 25. What is Lambda destination?

**Answer:**  
Destinations send invocation results to another service like:

- SQS
- SNS
- EventBridge  
  for success or failure handling.

---

## 26. What is Dead Letter Queue (DLQ)?

**Answer:**  
A queue (SQS/SNS) that stores failed events for later analysis or retry.

---

## 27. How does Lambda handle retries?

**Answer:**

- Synchronous: No retries by default
- Asynchronous: Automatic retries
- Stream-based: Retries until success or data expires

---

## 28. Lambda vs EC2?

**Answer:**

- **Lambda:** Serverless, auto-scaled, pay per execution
- **EC2:** Full control, long-running workloads, pay per hour

---

## 29. Lambda vs Kubernetes?

**Answer:**

- **Lambda:** Event-driven, stateless, no infra management
- **Kubernetes:** Container orchestration, full control, long-running services

---

## 30. When should you NOT use Lambda?

**Answer:**

- Heavy CPU workloads
- Stateful applications
- Ultra-low latency systems
- Long-running batch jobs
- Custom OS-level requirements

---

## 31. What is AWS Step Functions in relation to Lambda?

**Answer:**  
Step Functions orchestrate multiple Lambda functions into workflows with retries, branching, and error handling.

---

## 32. Can Lambda be versioned?

**Answer:**  
Yes. Lambda supports:

- Versions (immutable)
- Aliases (traffic shifting)

---

## 33. What is blue/green deployment in Lambda?

**Answer:**  
Using aliases to gradually shift traffic between Lambda versions.

---

## 34. Can Lambda run inside a container?

**Answer:**  
Yes. Lambda supports **container images up to 10 GB**.

---

## 35. What is the biggest advantage of Lambda?

**Answer:**  
Zero server management with automatic scaling and cost efficiency for event-driven workloads.

---

# END OF FILE
