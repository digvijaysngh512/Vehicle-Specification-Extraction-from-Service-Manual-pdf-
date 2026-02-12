# Vehicle Specification Extraction from Service Manual
## Objective
The goal of this assignment is to develop a basic system capable of retrieving vehicle specifications like torque values from an automotive service manual PDF document utilizing the technique of Retrieval-Augmented Generation (RAG) and Large Language Models.
The system deals only with texts and does not consider any diagrams that are presented in the manual.
### Tools & Libraries Used
•	Python
•	PyMuPDF (fitz)
•	Sentence-Transformers
•	FAISS
•	Hugging Face Transformers
•	LangChain Text Splitters
•	Pandas

## Pipeline Overview
The complete pipeline consists of the following stages:
### 1. PDF Text Extraction
The service manual pdf is parsed using PyMuPDF (fitz).
All pages are read and combined into a single raw text corpus.
### 2. Text Cleaning
Remove extra spaces, Normalize line breaks, Clean special symbols
This improves chunking and retrieval quality.
### 3. Text Chunking
RecursiveCharacterTextSplitter, Chunk size: 800, Overlap: 100
Chunking enables efficient semantic retrieval.
### 4. Embedding Creation
Each chunk is converted into vector embeddings using:
sentence-transformers/all-MiniLM-L6-v2
These embeddings represent semantic meaning of the text.
### 5. Vector Database (FAISS)
All embedding are stored in a FAISS index which allows similarity search between user queries and manual content.
### 6. Retrieval
For a given query (e.g., Brake caliper bolt torque), the system retrieves the associated chunks from the manual.
### 7. LLM-Based Extraction
Retrieved chunks are passed to an LLM:
google/flan-t5-large
The model attempts to extract structured specifications in the format:
Component | Spec Type | Value | Unit
### 8. Structured Output Processing
Since torque tables in the PDF are flattened during extraction, light post-processing is applied to: 
	Identify torque rows,
	Clean component names,
  Remove noise text,
  Structure the output,
### 9. CSV Export
## Example Query
Brake torque specifications

## Example Output

Component	Spec Type	Value	Unit
ABS module screws	Torque	3	Nm
Brake tube-to-HCU fittings	Torque	20	Nm

# Design Considerations
•	Used RAG architecture for contextual extraction
•	Chose lightweight embedding model for speed
•	Used open-source LLM for local execution
•	Applied minimal post-processing to structure flattened tables
## Limitations
•	Torque tables in PDFs lose column structure during text extraction
•	LLM struggles to interpret flattened numeric rows
•	Some noise specifications may appear without filtering
•	Fluid capacities and part numbers were not fully explored
## Future Scope of improvement 
•	Table-aware parsing
•	Multi-spec extraction (fluids, part numbers)
•	Better prompt engineering
•	UI interface for query input
•	Evaluation metrics for extraction accuracy
## Conclusion
The project successfully demonstrates a Retrieval-Augmented LLM pipeline capable of extracting structured vehicle specifications from service manual text and exporting them into usable tabular format.

