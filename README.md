[README.md](https://github.com/user-attachments/files/28840185/README.md)
# SCM Assistant Bot — Trinamix Hiring Task (TX-JrAI-003)

## 🔗 Public Chatbot URL
[https://cloud.flowiseai.com/chatbot/21aeca17-bcde-4154-ba6a-db64743cd384](https://cloud.flowiseai.com/chatbot/21aeca17-bcde-4154-ba6a-db64743cd384)

---

## 🛠 Tools & Models Used

| Component | Choice |
|---|---|
| Platform | Flowise Cloud (flowiseai.com) |
| LLM | Groq — llama-3.3-70b-versatile |
| Embeddings | Cohere Embeddings — embed-english-v3.0 |
| Vector Store | Qdrant Cloud |
| Chain | Conversational Retrieval QA Chain |

---

## 📁 Data Files

| File | Description |
|---|---|
| SupplyChain_Governance_Policy_v3.2.pdf | 10-section supplier governance policy |
| supplier_performance_data.csv | 2,000 purchase orders, 116 suppliers, 27 columns |

---

## 🧩 Chunk Configurations Tried

### Configuration 1 (Final)
- **Chunk Size:** 1000
- **Chunk Overlap:** 200
- **Splitter:** Recursive Character Text Splitter
- **Results:**
  - PDF: 19 chunks | 15,018 characters
  - CSV: 2,000 chunks (uploaded in 20 parts of 100 rows each)

### Configuration 2 (Experimented)
- **Chunk Size:** 500
- **Chunk Overlap:** 100
- **Splitter:** Recursive Character Text Splitter
- **Results:**
  - PDF: ~35 chunks (smaller chunks, more splits)
  - CSV: More granular chunks per row
- **Observation:** Smaller chunks produced more precise retrieval for specific supplier lookups but slightly increased noise for policy questions. Configuration 1 was chosen as the final setup for better balance.

---

## 💬 Sample Q&A (Verbatim Answers)

### Q1: Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?

**A:** 11 Tier-3 suppliers: Dravex Components India, Plataforma Metales SA, Maghreb Castworks, Helios Pack Greece, Cerromax Mineria, Orinoco Pack SAPI, Quetzal Textiles, Sibertek Molding, Archipelago PCB Corp, Varna Electronics EAD, Deltaforge Vietnam. All are High Risk with an active flag → Level 3 Activate per Policy §9 (CPO escalation + alternate supplier at minimum 40% volume).

---

### Q2: Which suppliers qualify for the annual Volume Rebate Program and how many are there?

**A:** 19 suppliers qualify: Borealis Composites, Crestline Chemical Supply, Fenwick Alloy Solutions, Hanguk Circuit Works, Hokkaido Alloy Tech, Krauss-Polymex GmbH, Lakeshore Components, Lumivex Semiconductor NL, Maplewood Polymer Corp, Norbec Alloy Works, Nordloom Finland Oy, Orrentek Precision Mfg, Ostwind Composites AG, PrecisionForge Taiyuan, Solveig Eco Packaging, Straits Packaging Hub, Tasman Circuit Boards, Toreval Electronics, Valdoro Special Alloys. Criteria (Policy §4.2): Tier-1 + OTD ≥ 93% + Defect < 0.5% + Sustainability Score ≥ 85.

---

### Q3: Which region has the highest total PO value, and does it breach the concentration limit?

**A:** EMEA at $193,987,179.91 — approximately 48.5% of total spend ($399,563,494.10). This breaches the 45% regional concentration cap (Policy §5.3), requiring a Diversification Plan within 60 days.

---

### Q4: Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?

**A:** 11 suppliers (Compliance Score < 60): Deltaforge Vietnam, Maghreb Castworks, Helios Pack Greece, Cerromax Mineria, Orinoco Pack SAPI, Varna Electronics EAD, Quetzal Textiles, Plataforma Metales SA, Archipelago PCB Corp, Dravex Components India, Sibertek Molding. SWL restricts new PO issuance to 20% of prior quarter volume (Policy §3.4).

---

### Q5: Which product category has the highest average defect rate and does it exceed the Tier-2 limit?

**A:** Mechanical Components — average 2.12% across 360 POs. Below the Tier-2 ceiling of 2.50% (Policy §3.2), so no breach — but approaching the limit.

---

## 🚀 What I Would Improve

1. **Upgrade to a paid vector store** — Qdrant free tier caused timeout issues with large CSV files. A paid plan would allow upserting all 2,000 rows at once without splitting.
2. **Use OpenAI Embeddings** — Cohere embeddings worked well but OpenAI text-embedding-3-small would provide better semantic accuracy for supply chain terminology.
3. **Add metadata filtering** — Tag each chunk with supplier name, tier, and region so the retriever can filter more precisely before passing to the LLM.
4. **Increase chunk overlap** — For policy documents, a higher overlap (300-400) would reduce the chance of missing context that spans chunk boundaries.
5. **Add a system prompt** — A custom system prompt instructing the LLM to always cite policy sections and supplier IDs would improve answer quality and consistency.
6. **Add memory/chat history** — Enable persistent memory so users can ask follow-up questions referencing previous answers in the same session.

---

## 📂 Repository Structure

```
scm-assistant-bot/
├── scm_assistant.json        # Exported Flowise chatflow
├── README.md                 # This file
├── .gitignore                # Excludes .env and API key files
└── screenshots/
    ├── 01_document_store.png
    ├── 02_upsert_success.png
    ├── 03_chatflow_canvas.png
    ├── 04_share_chatbot.png
    └── 05_chat_responses.png
```

---

*Submitted by: Shubham Dambal | Ref: TX-JrAI-003*
