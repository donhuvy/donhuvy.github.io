---
layout: post
title:  "Natural language processing by Python"
date:   2021-01-17 22:30:00 +0700
categories: Python NLP
---
Natural language processing by Python

```python
import PyPDF2
from PyPDF2 import PdfFileReader

pdf = open("abc.pdf", "rb")
pdf_reader = PyPDF2.PdfFileReader(pdf)

print(pdf_reader.numPages)
page = pdf_reader.getPage(0)

print(page.extractText())

pdf.close()
```