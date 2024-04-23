## Ravi Patel

### Objective: 

- Get dwelling and total premium from pdfs

### Notes:

- Here, the decision was made to bypass attempting the search for the page with the keyword because of limited time. 


### Plan:     
1. Split pdfs into images
2. Use either pytesseract or easyocr to extract text
3. Use jellyfish to compare text to keywords
4. If keywords are found, then attempt to gather only numbers from rows. 


### Improvements:
- Attempt to index all of the text for search later to find page to focus on.
- Ingest data from pdfs into a vector database and use AI query to find data. 
- If working with PDF, attempt to parse tree. This solution loses significant amounts of information given in PDF format.
- Check viability of zeroshot model to query image.
- Evaluate CV models to find specific matches.

### Useful libraries:
- https://pypi.org/project/PyMuPDF/
- https://pypi.org/project/pytesseract/
- https://github.com/JaidedAI/EasyOCR
- https://opencv.org
- https://pypi.org/project/pillow/
- https://pypi.org/project/jellyfish/