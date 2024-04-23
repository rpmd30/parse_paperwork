## Ravi Patel

### Running instructions:
- Run `virtualenv venv`
- Run `source venv/bin/activate`
- Run `pip install -r requirements.txt`
- Copy `auto-9.pdf` and `auto19.pdf` to root directory of project folder
- Run `jupyter notebook`
- Once browser is open, open `analyze_pdfs.ipynb` notebook file

### Objective: 

- Get dwelling and total premium from pdfs

### Notes:

- Here, the decision was made to bypass attempting the search for the page with the keyword because of limited time. 


### Plan:     
1. Split pdfs into images
2. Use either pytesseract or easyocr to extract text
3. Use jellyfish to compare text to keywords
4. If keywords are found, then attempt to gather only numbers from rows. 


### Scratch pad techinques attempted which didn't yield satisfactory results in timeline required:
- opencv match template + minMaxLoc
- PyMuPDF search for keywords
- OWL v2 zero shot transformer model
- Other OCR search techniques

### Improvements and experiments:
- Attempt to index all of the text for search later to find page to focus on.
- If working with PDF, attempt to parse tree. This solution loses significant amounts of information given in PDF format.
- Ingest data from pdfs into a vector database and use AI query to find data. 
- Check viability of zeroshot model to query image (See OWL-ViT with image query)
- Evaluate CV models to find specific matches 
    - See using cascading classifier
    - match template
- Reverse engineer other search keywords which could be misclassified
    - This could be used to train a new model to find potential misclassifications. 

### Scaling
- Depending on algorithm requirements such as GPUs and round trip time that is needed there are a few options(any option will require profiling and resource monitoring before making decision):
    - Batch processing at set intervals to ensure GPUs are saturated and not running constantly to minimize cost
    - Utilize CPU only options to process data in stream
    - Utilize Vertex or sagemaker to host models to ensure pipelines can be autoscaled when needed

### Data type considerations:
- The implemented solution is most likely the worst case scenario that can be encountered as PDF files can contain significantly more information regarding structure. A more robust solution would attempt to parse pdf file first and attempt to find information via tree and heuristics before utilizing AL/ML which can be more expensive operations.

### Technologies considered:
- Transformer models with Pytorch
- Manual multimodal models with Pytorch (Image + Text)
- Ollama + vector database(Chroma?) for in house AI models
- Additional OCR libraries can enable:
    
    - The text can be used in multiple avenues from verifying that the page selected has keywords which need to be verified.
    - The text can also be used as input into AI model for querying for information.
- OpenCV options
- AI service if data is not sensitive (langchain + ChatGPT will also attempt to input data from PDFs, but usefulness needs to be tested in this scenario)

### Option 1:
- Break problem down into choosing the page of interest for data. This could utilize NLP type solution or CV solution to understand what information is on page to determine level of importance of page.
- Then once on page, utilize CV/OCR to determine if it is possible to narrow down regions of interest. Terminology and structure will be different from customer to customer along with quality of files and etc. A second model could be utilized here to determine what information is needed to be gathered from patterns from previous data.
- Once data is gathered, store information in structured format to be validated for QA. 
- In future algorithms, run tests against validated data. While expensive, building this infrascture out early can be extremely valuable. 
- Pros: There is room to control each level of the stack and can enable more modular design. The individual pieces will also increase visibility when debugging. There is more room to optimize for cost. There are also many tools available which have been tried and tested.
- Cons: This pipeline is intricate to manage and may not be as flexible as other options.

### Option 2:
- Attempt to utilize AI heavily in house to gather all information possible from PDF using fine tuning or RAG structure. 
- Pros: The potential is quite high to have a flexible pipeline that can be more user/developer friendly once setup. This can enable non-developers to create query mappings.
- Cons: Model is a black box. Cost maybe higher than option 1. The model could be more suseptiable to bad data unless additional guard rails are put in place. 

### Option 3:
- Utilize 3rd party service such as Google Document AI or OpenAI to manage PDF processing and querying.
- Pros: Easier to setup initially. Documentation could be better than other options.
- Cons: Lack of control over technical stack. 3rd party pricing can change over time. Models could change over time which could cause headaches.

### Testing objectives:
- Data which is already validated and labeled will be the ideal testing dataset for any new model or algorithms added to a system. There are various metrics used to verify new algorithms and models are improvements over previous structures for example tracking F1 scores can be a good baseline for tracking model behavior. This should also be monitored as models in production can drift over time which can be detrimental to service if not maintained properly. 

### Final Thoughts:
- There are many options and adjustments that can be made from here. This is just the first step in a long journey to continue to improve customer experience. The goal here is to have an initial step to begin to move and learn. 

### Useful libraries:
- https://pypi.org/project/PyMuPDF/
- https://pypi.org/project/pytesseract/
- https://github.com/JaidedAI/EasyOCR
- https://opencv.org
- https://pypi.org/project/pillow/
- https://pypi.org/project/jellyfish/