# Traditional vs. Modern Workflow Orchestration

- [Traditional vs. Modern Workflow Orchestration](#traditional-vs-modern-workflow-orchestration)
  - [1. Traditional Workflow Orchestration Methods](#1-traditional-workflow-orchestration-methods)
    - [1.1 Database-Driven Workflows](#11-database-driven-workflows)
      - [How it Worked](#how-it-worked)
      - [Challenges](#challenges)
      - [Example SQL Schema](#example-sql-schema)
    - [1.2 Message Queue-Based Workflows](#12-message-queue-based-workflows)
      - [How it Worked](#how-it-worked-1)
      - [Challenges](#challenges-1)
      - [Example Process Flow](#example-process-flow)
      - [Example RabbitMQ Publishing](#example-rabbitmq-publishing)
    - [1.3 Cron Job-Based Processing](#13-cron-job-based-processing)
      - [How it Worked](#how-it-worked-2)
      - [Challenges](#challenges-2)
      - [Example Cron Job Setup](#example-cron-job-setup)
      - [Example Python Script for Pending Workflows](#example-python-script-for-pending-workflows)
    - [1.4 Hybrid Approach (DB + Queues + Workers)](#14-hybrid-approach-db--queues--workers)
      - [How it Worked](#how-it-worked-3)
      - [Example Process](#example-process)
      - [Challenges](#challenges-3)
  - [2. Modern Workflow Orchestration with Temporal](#2-modern-workflow-orchestration-with-temporal)
    - [2.1 How Temporal Works](#21-how-temporal-works)
      - [Key Components](#key-components)
      - [Steps in Card Issuance Workflow](#steps-in-card-issuance-workflow)
      - [Temporal Workflow Implementation (Python)](#temporal-workflow-implementation-python)
    - [2.2 Temporal Alternatives](#22-temporal-alternatives)
    - [2.3 Why Temporal is the Best Choice](#23-why-temporal-is-the-best-choice)
  - [3. Conclusion](#3-conclusion)

## 1. Traditional Workflow Orchestration Methods

Before modern workflow orchestration platforms like **Temporal, Camunda, or Airflow**, developers built custom solutions using **databases, message queues, cron jobs, and manual state management**. Below are the common approaches used.

### 1.1 Database-Driven Workflows

#### How it Worked

- Each step of the workflow was stored in a **database table** (e.g., `workflows` table).
- A **background polling job (cron job or worker)** periodically checked for workflows in a "pending" state.
- When an external event happened (e.g., **KYC completion**), an **update query** was triggered to mark the next step as active.
- The **background job picked up the next step** and executed it.

#### Challenges

❌ **Inefficient Polling**: The system had to keep checking the database for pending workflows.  
❌ **Scaling Issues**: Too many workflows led to high database load.  
❌ **Failure Handling**: If a job failed, someone had to manually restart or write custom retry logic.  
❌ **Complex State Management**: Manual tracking of workflow progress.

#### Example SQL Schema

```sql
CREATE TABLE workflows (
    id SERIAL PRIMARY KEY,
    customer_id UUID NOT NULL,
    step VARCHAR(50) NOT NULL,
    status VARCHAR(20) CHECK (status IN ('pending', 'in_progress', 'completed', 'failed')),
    created_at TIMESTAMP DEFAULT now(),
    updated_at TIMESTAMP DEFAULT now()
);
```

A **cron job** checked for pending workflows and processed them.

---

### 1.2 Message Queue-Based Workflows

#### How it Worked

- Instead of polling a database, a **message queue** (RabbitMQ, ActiveMQ, SQS, Kafka) was used.
- When a step was completed, an event (message) was **pushed to a queue**.
- Background workers **listened to the queue** and executed the next step.
- Failed tasks were **pushed to a Dead Letter Queue (DLQ)** for retries.

#### Challenges

❌ **No Global State Management**: Hard to track workflow progress.  
❌ **Duplicate Processing**: Extra safeguards were needed to prevent reprocessing.  
❌ **Hard to Debug & Monitor**: No centralized view of workflow progress.

#### Example Process Flow

1. **Customer onboarded** → Message sent to `kyc_pending_queue`
2. **KYC Completed** → Message sent to `card_issuance_queue`
3. **Card Issued** → Message sent to `card_limit_queue`

#### Example RabbitMQ Publishing

```python
channel.basic_publish(exchange='',
                      routing_key='kyc_pending_queue',
                      body=json.dumps(customer_data))
```

---

### 1.3 Cron Job-Based Processing

#### How it Worked

- **Cron jobs** ran at **fixed intervals** to check for incomplete workflows.
- They called **APIs** or processed **database entries** to move workflows forward.

#### Challenges

❌ **Not real-time**: Jobs ran on a fixed schedule, causing delays.  
❌ **Hard to scale**: Too many workflows caused delays in execution.  
❌ **Manual Retries**: Developers had to write custom retry logic.

#### Example Cron Job Setup

```bash
*/5 * * * * python process_workflows.py
```

#### Example Python Script for Pending Workflows

```python
def process_pending_workflows():
    workflows = db.fetch("SELECT * FROM workflows WHERE status='pending'")
    for wf in workflows:
        process_workflow(wf)
```

---

### 1.4 Hybrid Approach (DB + Queues + Workers)

#### How it Worked

- Used a **database** for tracking workflow state.
- Used a **message queue** to push tasks **only when needed**.
- Workers processed tasks asynchronously.

#### Example Process

1. **Partner API** stores onboarding request in DB.
2. **KYC is incomplete** → Background worker waits for an event.
3. **Once KYC is complete** → Workflow updates DB and triggers a message queue.
4. **Card issuance service listens for messages** and issues a card.
5. **Final worker assigns card limits** and marks the process complete.

#### Challenges

❌ **Still manual retries** – needed custom logic.  
❌ **Multiple moving parts** – hard to debug failures.

---

## 2. Modern Workflow Orchestration with Temporal

### 2.1 How Temporal Works

Temporal provides a **stateful, durable, and fault-tolerant** way to manage workflows. It ensures that **each step is tracked, retried, and resumed from failures**.

#### Key Components

- **Workflows**: Define the sequence of steps.
- **Activities**: External calls (e.g., APIs, database operations).

#### Steps in Card Issuance Workflow

1. **Partner Onboards Customer** → Starts a workflow.
2. **Wait for KYC Completion** (indefinite wait).
3. **Trigger Card Issuance** (handles failures with retries).
4. **Set Card Limit** (final step, failure handling).

#### Temporal Workflow Implementation (Python)

```python
import temporalio.workflow
from activities import start_kyc, issue_card, set_card_limit

@temporalio.workflow.defn
class CardIssuanceWorkflow:
    @temporalio.workflow.run
    async def run(self, customer_id: str):
        await temporalio.workflow.execute_activity(start_kyc, customer_id)
        await temporalio.workflow.wait_signal("kyc_completed")
        card_id = await temporalio.workflow.execute_activity(issue_card, customer_id)
        await temporalio.workflow.execute_activity(set_card_limit, card_id)
        return f"Card issued for customer {customer_id}"
```

---

### 2.2 Temporal Alternatives

| Workflow Engine  | Pros | Cons |
|-----------------|------|------|
| **Cadence (Uber)** | Scalable, mature | Harder to set up than Temporal |
| **Apache Airflow** | Good for batch workflows | Not event-driven, polling-based |
| **Camunda Zeebe** | BPMN-based visual workflows | Complex setup |
| **AWS Step Functions** | Serverless, managed by AWS | Tied to AWS |
| **Netflix Conductor** | Built for microservices | Complex setup |

---

### 2.3 Why Temporal is the Best Choice

✅ **Stateful, Event-Driven** → Waits indefinitely for KYC without polling.  
✅ **Automatic Retries** → Handles API failures without manual retries.  
✅ **Fault-Tolerant** → Resumes from the last successful step.  
✅ **Scalability** → Handles **millions of workflows efficiently**.  
✅ **Observability** → UI to track where workflows are stuck.

---

## 3. Conclusion

Older workflow methods relied on **databases, queues, and cron jobs**, leading to **scalability and reliability issues**. **Modern workflow orchestration tools like Temporal** solve these challenges by handling **state, retries, and failures automatically**. 🚀
