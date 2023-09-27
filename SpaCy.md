- spaCy is a **free, open-source library** for advanced **Natural Language Processing** (NLP) in Python.
- spaCy is designed specifically for **production use** and helps you build applications that process and “understand” large volumes of text. It can be used to build **information extraction** or **natural language understanding** systems, or to pre-process text for **deep learning**.

### POS

```python
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("Apple is looking at buying U.K. startup for $1 billion")

for token in doc:
    print(token.text, token.lemma_, token.pos_, token.tag_, token.dep_,
            token.shape_, token.is_alpha, token.is_stop)
```

### NER

```python
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("Apple is looking at buying U.K. startup for $1 billion")

for ent in doc.ents:
    print(ent.text, ent.start_char, ent.end_char, ent.label_)

```

## How can I customize NER in spaCy for my use case?

1. Add rule-based matching

- Useful if there’s a finite number of examples that you want to find in the data, or if there’s a very clear, structured pattern
- For instance, country names, IP addresses or URLs

2. Train a custom model

- Useful if you have examples and you want your system to generalize based on those examples
- Works especially well if there are clues in the local context
- For instance, person or company names

You can also combine both approaches and improve a model with rules to handle very specific cases and boost accuracy.

# Resources
- jupyter notebook: spacy training Data - > https://nbviewer.org/github/kriesbeck/spacy-ner/blob/master/NER%20in%20spaCy.ipynb
