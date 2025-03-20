# Best Practices for Leveraging Azure OpenAI in Constrained Optimization Scenarios

*Authors:*
- *Ellie Nosrat, Principal AI Partner Solution Architect*
- *Lauren Tran, Principal AI Partner Solution Architect*
- *Melody Yin, AI Partner Solution Architect*

Constrained optimization problems come up in various fields and range from scheduling and logistics to financial planning and resource allocation. By leveraging Generative AI (GenAI) to solve these complex decision-making tasks, organizations can become more efficient and productive in their operations. 

In this article, we will outline best practices for using GenAI in constrained optimization, using a real-world example: an AI-powered college course scheduling solution. We will also explore other applications, such as workforce scheduling and supply chain management.

---

## Understanding Constrained Optimization in AI

Constrained optimization involves finding the best solution to a problem while satisfying a set of predefined constraints. These constraints could be based on rules, resources, or preferences. Traditional optimization techniques, such as linear programming, evolutionary algorithms, and constraint satisfaction solvers, have been historically used for these tasks. In today's AI landscape, we find that GenAI can significantly accelerate solution development in this problem domain.

However, successfully integrating GenAI into constrained optimization requires careful consideration of trade-offs, architecture choices, and mitigation of potential failures like hallucination.

---

## Real-World Example: AI-Powered Course Scheduling

Let’s consider a specific scenario – **AI-assisted scheduling for college courses** – to illustrate concepts, implementation, and best practices for solving a constrained optimization problem with **Azure OpenAI o-series reasoning models**. 

For this use case, GenAI can help recommend courses based on major, course prerequisites, student data, and student preferences. The following architecture illustrates implementing a course scheduling system with a multi-agent solution, leveraging the **Reflection pattern** outlined here: [Microsoft AutoGen Reflection](https://microsoft.github.io/autogen/stable/user-guide/core-user-guide/design-patterns/reflection.html).

The following architecture diagram illustrates an example implementation of this solution, where Azure SQL Database is leveraged to store student information and course data.

![Architecture Diagram](/assets/constrained_optimization.png)


> 🔎 **Note:** Real-world optimization scenarios often involve more constraints than those shown in simplified examples. When developing a proof-of-concept, we recommend starting with a scoped problem that highlights the most impactful variables and constraints. This approach accelerates learning and allows stakeholders to evaluate solution viability before expanding to full production complexity.

The steps for solving this course scheduling problem are as follows:

### 1. User Query
The student provides details such as their major, number of courses they want to take, and preferences (e.g., _“I am a computer science major, and I'd like to take 3 major courses and 1 elective next semester. I prefer morning and in-person courses”_).

### 2. Scheduling Agent
The student’s request gets routed to the first agent &mdash; the scheduling agent &mdash; which queries a datastore for available courses. With the appropriate grounding data, the agent sends the student query and courses to **Azure OpenAI** to generate a course schedule.

#### System Message:
```
You are a scheduler. You recommend a student's schedule based on the courses provided.
Work with the reviewer to ensure your proposed schedule has no conflicts and aligns with the student's major and preferences.

If there is no course combination that work, do not make up courses or times.
Respond that you cannot meet the requirements, provide a reason, and suggest alternative courses.
```

### 3. Reviewer Agent
To ensure accuracy and that there are no course conflicts while adhering to student preferences, the reviewer agent considers the proposed schedule and sends feedback to the scheduling agent. The reviewer may either approve or request a revision to the schedule while providing feedback to the scheduler.

#### System Message:
```
You are a schedule reviewer. You ensure there are no time conflicts in the schedule created by the scheduler agent.

You fact check the proposed course names, days, and times against the data provided to ensure validity.

Try to ensure the student's preferences are taken into consideration, but prioritize conflicts and accuracy first.

Respond using the following JSON format: 
{ 
    "approval": "<APPROVE or REVISE>", 
    "suggested_changes": "<Your comments>" 
}
```


### 4. Design Trade-offs
- **Model Selection**: While **GPT-4o** produces faster results, **o3-mini** shows better performance in constrained scheduling tasks, reducing hallucinations and improving accuracy.
- **Predefined SQL queries vs. AI-generated queries**: Since this system is fairly static, predefined SQL queries ensure higher accuracy and lower latency compared to AI-generated queries. However, in a more dynamic system, an additional **Course Retriever Agent** could be used to generate SQL queries dynamically.

---

## Best Practices for Using GenAI in Constrained Optimization

### 1. Select the Right Model 
- Use **AI models optimized for reasoning tasks** rather than purely generative tasks.
- Experiment with different models to balance **accuracy and performance**.
- Consider models that explicitly handle constraints well, such as **Azure OpenAI’s o-series**.

### 2. Use Prompt Engineering to Minimize Errors
- AI models tend to hallucinate when constraints make the problem infeasible.
- Explicitly instruct AI to acknowledge constraints and suggest alternatives rather than fabricating results.
- **Example prompts**:
  - _"Do not generate new data. If constraints are too strict, indicate no valid solution exists and provide alternatives."_
  - _"Validate all outputs against predefined constraints before returning results."_

### 3. Keep It Simple: Leverage AI Only Where Needed
- If SQL queries or other deterministic methods can solve part of the problem efficiently, use them instead of AI.
- AI should augment structured logic, not replace it unnecessarily.
- **Example**: For course scheduling, predefined SQL queries for retrieving courses where the student has satisfied the prerequisites ensure faster, more accurate results than AI-driven query generation.

### 4. Use Multi-Agent Architectures for Better Constraint Handling
- Split complex optimization tasks between specialized agents:
  - **Proposal Agent**: Generates initial solutions.
  - **Validation Agent**: Ensures feasibility and compliance.
- This reduces the likelihood of errors and ensures that AI follows structured logic.

---

## Additional Examples of Constrained Optimization

### Workforce Scheduling
- **Constraints**: Employee availability, labor laws, shift preferences, business hours.
- **Best Practice**: Combine AI recommendations with rule-based verification for compliance.

### Supply Chain Management
- **Constraints**: Inventory levels, supplier lead times, demand forecasts, cost limits.
- **Best Practice**: Use AI to optimize for multiple objectives (cost vs. speed) while ensuring feasibility.

---

## Conclusion

GenAI can be applied in constrained optimization across diverse industries. However, to maximize its effectiveness, AI practitioners must:

- **Choose appropriate AI models** for the task.
- **Use prompt engineering** to minimize errors and hallucinations.
- **Integrate deterministic solutions** where applicable.
- **Structure AI-driven solutions** with multi-agent architectures.
- **Continuously monitor and refine AI performance**.

By following these best practices, businesses can leverage AI to solve complex optimization challenges efficiently and reliably. 