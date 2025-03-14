# Best Practices for Requesting Quota Increase for Azure OpenAI Models  

*Author: Ellie Nosrat, Principal AI Partner Solution Architect*

---

This document outlines a set of best practices to guide users in submitting quota increase requests for Azure OpenAI models. Following these recommendations will help streamline the process, ensure proper documentation, and improve the likelihood of a successful request.  

## 1. Understand the Quota and Limitations  

Before submitting a quota increase request, make sure you have a clear understanding of:  

- The current quota and limits for your Azure OpenAI instance.  
- Your specific use case requirements, including estimated daily/weekly/monthly usage.  
- The rate limits for API calls and how they affect your solution's performance.  

Use the Azure portal or CLI to monitor your current usage and identify patterns that justify the need for a quota increase.  

---

## 2. Provide a Clear and Detailed Justification  

When requesting a quota increase, include a well-documented justification. This should include:  

### Use Case Description  
Provide an overview of how you are using Azure OpenAI services, including details about the application or platform it supports.  

### User Impact  
Explain how the quota increase will benefit end-users or improve the solution's performance. Provide details of **ACR impact**, if possible.  

### Current Limits vs. Required Limits  
Clearly state your current quota and the requested increase. For example:  
> **Current limit:** 10,000 tokens per minute  
> **Requested limit:** 50,000 tokens per minute  

### Supporting Data  
Include quantitative data such as:  
- **Historical usage trends**  
- **Growth projections** (e.g., user base, API calls, or token consumption)  
- **Expected peak usage scenarios** (e.g., seasonal demand or events)  

---

## 3. Follow Prompt Engineering Best Practices  

As a first step, ensure you are optimizing your usage of Azure OpenAI models by adhering to **prompt engineering best practices**:  

- **Plain Text Over Complex Formats** – Use simple, clear prompts to minimize errors and improve model efficiency.  
- **JSON Format (if applicable)** – If structured data is required, experiment with lightweight and efficient JSON schemas.  
- **Clear Instructions** – Provide concise and unambiguous instructions in your prompts to reduce unnecessary token consumption.  
- **Iterative Refinement** – Continuously refine prompts to strike a balance between response quality and token usage.  

---

## 4. Optimize API Usage  

Ensure that your solution is designed to optimize API usage:  

- **Batch Requests/Processes** – Combine multiple smaller requests into a single larger request where possible, or use the batch endpoint option in Azure OpenAI.  
- **Rate-Limit Handling** – Implement robust retry mechanisms and backoff strategies to handle rate-limited responses gracefully.  
- **Caching Responses** – Cache frequently requested results to minimize redundant API calls.  

---

## 5. Include Architectural and Operational Details  

If your request involves high usage or complex architecture, provide additional details:  

- **System Architecture Overview** – Describe the architecture of your solution, including how Azure OpenAI services integrate with other platforms (e.g., AWS, on-premises systems, etc.).  
- **Rate Limiting Strategy** – Highlight any strategies in place to manage rate limits, such as prioritizing critical requests or implementing queuing mechanisms.  
- **Monitoring and Alerts** – Share your approach to monitoring API usage and setting up alerts for potential issues.  

---

## 6. Submit with Supporting Documentation  

Prepare and attach any supporting materials that can strengthen your case:  

- **Usage Reports** – Include graphs, charts, or tables showing current consumption trends.  
- **Excel Calculations** – Provide detailed calculations related to rate limit requirements, if applicable.  
- **Screenshots or Diagrams** – Share visuals that demonstrate your use case or architectural setup.  
- **Concise and Structured Format** – Use bullet points or numbered lists for clarity.  
- **Alignment with Business Goals** – Connect the request to business outcomes, such as improved customer experience or scalability for growth.  
- **Free of Ambiguity** – Avoid vague language; provide specific details wherever possible.  
