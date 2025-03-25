# Best Practices for Structured Extraction from Documents Using Azure OpenAI

## Overview

In a recent project, a customer needed to extract structured data from legal documents to populate a standardized form. The legal documents varied in length and structure, and the customer required consistent and accurate outputs that mapped directly to the expected form schema. The implemented solution leveraged Azure OpenAI to iteratively process document chunks and update the form output dynamically. A key component to successfully extract the correct output was using **Structured Outputs** to enforce the desired output fields to populate the form.

This article outlines best practices derived from this project, with a focus on scalable methods, reliable structure enforcement, and iterative processing of unstructured legal data.  These lessons learned can be leveraged for additional scenarios and document types beyond legal documents in various industries including:

- Healthcare: patient record management, clinical data extraction
- Finance: automated processing of financial statements, regulatory reporting
- Insurance: claims processing, policy management
- Supply chain logistics: extracting shipment details and tracking information from shipping documents

---

## 1. Analyze Inputs: Document Characteristics and Requirements

- **Input Types**: Unstructured legal documents (contracts, disclosures, etc.) with embedded answers to a pre-defined structured form.
- **Challenges Identified**:
  - Information was scattered and not linearly presented.
  - Repetition or rephrased potential answers in different sections.
  - Inconsistent field presence across documents.
  - LLMs occasionally hallucinated or restructured the desired output schema.

---

## 2. Key Practices That Drove Success

### Chunked Document Processing

- **Approach**: Split long legal documents into manageable text chunks, even if full document fits in context window.
- **Loop Strategy**: Sequentially pass each chunk to the LLM, checking if any structured field should be updated based on new context.
- **Why it works**: Allows the model to refine or reinforce fields incrementally, rather than relying on a single-shot generation.

### Schema Enforcement with Structured Outputs + Pydantic

- **Approach**: Use Azure OpenAI’s support for structured outputs defined via Pydantic models.
- **Benefits**:
  - Prevents hallucination of new or irrelevant fields.
  - Preserves field order and hierarchy as defined by the form schema.
  - Eliminates risk of nested or malformed structures that deviate from the form expectations.

Here’s an example of a `Pydantic` model used in this context:

```python
from pydantic import BaseModel, Field

class ContractData(BaseModel):
    contract_title: str 
    contract_date: str 
    party_1_name: str 
    party_2_name: str 
    effective_date: str 
    term_length: str 
    payment_terms: str 
    termination_clause_summary: str
    governing_law: str 
    implicit_obligations: str
```
- This schema clearly defines the expected fields.
- Any deviation from this structure during LLM inference will result in a validation error, ensuring consistency and control.
- Each chunk of the document can be passed along with this model to validate and extract data in a structured, reliable way.

### Update-on-Change Logic

- **Approach**: If a field’s value remains consistent across chunks, retain it. If a chunk contradicts or supplements earlier info, update the field.
- **Why it works**: Balances accuracy with efficiency, enabling LLMs to focus only on relevant changes.

---
The following diagram demonstrates the iterative workflow of reading and updating based on new context until generating the final structured output:

![Structured data extraction](/assets/structured_data_extraction.png)

---

## 3. Synthesized Best Practices for Broader Application

| **Best Practice**                        | **Recommendations**                                                                 | **Why It Matters**                                                                 |
|------------------------------------|----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| **Use Structured Outputs** | Define a strict schema using Pydantic models and pass it to Azure OpenAI via the `response_format` parameter. | Prevents field drift and ensures clean, valid output that aligns with the target form. |
| **Chunk the Input Intelligently**   | Split documents by semantic or paragraph boundaries to maintain contextual coherence.        | Helps the model focus on relevant information and reduces token limitations.         |
| **Iterative Form Updating**         | After each chunk, compare and update fields only when new data is more accurate or complete. | Reduces noise and leverages cumulative document context effectively.                 |
| **Avoid Free-Form Generation**      | Do not allow the model to generate the full form output from scratch unless necessary.       | Free-form outputs risk inconsistencies, omissions, and hallucinations.               |

---

This workflow is especially powerful when applied to legal, compliance, or financial documents where:

- Structure matters more than narrative coherence.
- Small errors in field names or formats can break downstream automation.
- Information is cumulative and scattered.

By combining chunked processing with schema enforcement, this approach transforms variable format unstructured text into a structured and consumable output, enabling downstream automation.

---

## Conclusion

By adopting these best practices—particularly leveraging Azure OpenAI iteratively with schema enforcement through Structured Outputs—organizations can achieve highly accurate and dependable structured data extraction from complex, unstructured documents. This approach ensures that automated processing is robust, consistent, and ready for successful downstream application.