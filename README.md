# EC2 Automatic Start & Stop Using AWS Scheduler 
(Detailed Step-by-Step Notes for GitHub) 


---

# 1. Objective - 

To automatically start and stop AWS EC2 instances at predefined times using AWS managed services in order to reduce infrastructure cost.

Scheduled time windows (IST):

10 AM – 2 PM

11 AM – 1 PM

7 PM – 10 PM

---

# 2️. AWS Services Used

Amazon EC2 – Compute instances

AWS Lambda – Serverless execution

Amazon EventBridge – Time-based scheduling

AWS IAM – Permissions management

---

# 3️. Overall Working Flow

EventBridge (Cron Schedule)
        
Lambda Function
        
EC2 Instances Start / Stop

---

# 4️. Step 1: EC2 Instance
- Create 3 EC2 Instances 
- Open Amazon EC2 
- Click Launch instance 
- Create three separate EC2 instances

<img width="1920" height="818" alt="image" src="https://github.com/user-attachments/assets/56178c32-6ae7-4925-a0ef-7e05f61a339c" />

  ---

 # 5️. Step 2: Create IAM Role for Lambda

## Lambda requires permissions to manage EC2 and write logs.

Steps :

- Go to IAM → Roles → Create Role

- Trusted entity: AWS Service

- Use case: Lambda

<img width="1920" height="823" alt="image" src="https://github.com/user-attachments/assets/3f189650-bd78-4d7c-8cd1-35ca1f8a87e3" />



- Attach policies:
```
AmazonEC2FullAccess
```

- Role name:

```

Lambda-EC2-Start-Stop-Role

```
<img width="1906" height="832" alt="image" src="https://github.com/user-attachments/assets/e79382d7-9030-441d-9e4d-b727a60e2487" />

---

# 6.Step 3: Create Lambda Function for EC2 Start and Stop 
1. Open AWS Lambda 
2. Click Create function 
3. Select Author from scratch 
4. Configure: 
  - Function name: EC2-Start-Stop 
  - Runtime: Python 3.12 
  - Architecture: x86_64 
5. Under Permissions: 
  - Select Use an existing role 
  - Choose the IAM role with EC2 start/stop permissions 
6. Click Create function

<img width="1920" height="824" alt="image" src="https://github.com/user-attachments/assets/c3a46c1e-73b7-4f20-9e25-683bbcb426c5" />

---

# 7. Step 4:  Create EventBridge Rule for Starting Instances
 
1. Open Amazon EventBridge 
2. Click Create schedule 
3. Configure: 
o Name: Start-ec2 10 am
o Schedule type: Recurring 
o Schedule pattern: Cron 
o Cron (UTC) 
4. Flexible time window: Off 
5. Target:
    - Service: Lambda 
    - Function: EC2-Start-Stop


 <img width="1903" height="819" alt="image" src="https://github.com/user-attachments/assets/a0fafc86-32ec-41f9-989e-e87fc4dcd93f" />
   
6. Input JSON

```

{ 
  "action": "stop", 
  "schedule": "lambda-server 1" 
}

{ 
  "action": "stop", 
  "schedule": "lambda-server 1" 
}

```
---
  
 8. Create schedules using the same Lambda function:

###  Schedule Overview

| Schedule Name | Instance Name | Action | Time (IST) |
|--------------|---------------|--------|------------|
| start-lambda-server-1 | lambda-server-1 | Start | 10:00 AM |
| stop-lambda-server-1 | lambda-server-1 | Stop | 02:00 PM |
| start-lambda-server-2 | lambda-server-2 | Start | 11:00 AM |
| stop-lambda-server-2 | lambda-server-2 | Stop | 01:00 PM |
| start-lambda-server-3 | lambda-server-3 | Start | 07:00 PM |
| stop-lambda-server-3 | lambda-server-3 | Stop | 10:00 PM |

---
# 9. Step 5. verify


<img width="1908" height="823" alt="image" src="https://github.com/user-attachments/assets/8e4c4256-35a1-411a-a2a3-a285013b2700" />

---

## COST OPTIMIZATION BENEFITS

 No manual EC2 management
 
- Cost-efficient infrastructure
- Production-ready scheduling
- Works for multiple instances
- EC2 instances run **only during required business hours**
- Eliminates idle compute costs during off-hours
- Scalable solution for production environments
---
###  Key Learnings

- Real-world usage of EventBridge Scheduler
- Deep understanding of IAM trust relationships
- Lambda integration with EC2 APIs
- Production-level scheduling patterns
- Cost-aware cloud architecture design
---






