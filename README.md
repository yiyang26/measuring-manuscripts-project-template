# From XML to Entities

## Exploring Named Entity Recognition on *Repertorium Germanicum I*

This Digital Philology project explores how three pretrained named entity
recognition (NER) models respond to the Latin regesta of
*Repertorium Germanicum I* (RG I).

**Live project website:**  
https://yiyang26.github.io/RG1-NER-exploration/

## Research Question

How do NER models trained in different linguistic and historical contexts
respond to the abbreviations, historical names, editorial structure, and
compressed language of RG I?

The project compares model outputs rather than accuracy. RG I has no
gold-standard NER annotations, so precision, recall, and F1 cannot be
calculated reliably.

## Corpus

The [Repertorium Germanicum Online](https://rg-online.dhi-roma.it/denqRG/index.htm),
produced by the German Historical Institute in Rome, records people, churches,
places, and legal or ecclesiastical actions found in Vatican registers and
cameral sources.

RG contains regesta rather than complete document transcriptions. These are
concise scholarly summaries containing dense historical information and
frequent abbreviations.

The XML extraction produced:

| Unit | Number |
|---|---:|
| Lemmas | 3,845 |
| Head segments | 3,845 |
| Sublemma segments | 5,589 |
| Total segments | 9,434 |

Abbreviations occur in 8,288 segments, or 87.9% of the corpus.

## Workflow

The project consists of three stages:

1. Parse the RG I XML and extract heads, sublemmata, abbreviations, dates, and source references.
2. Prepare the texts and apply three pretrained NER models to all 9,434 segments.
3. Compare model outputs and examine selected cases qualitatively.

Structured source references were removed from the NER input. Original spelling,
punctuation, dates, and abbreviations were preserved.

## NER Models

### LatinCy

[`latincy/la_core_web_lg`](https://huggingface.co/latincy/la_core_web_lg)
is a general-purpose Latin spaCy pipeline used as the Latin-specific baseline.

Labels in this project: `PERSON`, `LOC`, `NORP`.

### Multilingual Medieval RoBERTa

[`magistermilitum/roberta-multilingual-medieval-ner`](https://huggingface.co/magistermilitum/roberta-multilingual-medieval-ner)
was fine-tuned on approximately 8,000 medieval charter texts in Medieval Latin,
Old French, and Old Spanish.

Labels in this project: `PERS`, `LOC`.

### Multilingual XLM-R

[`Davlan/xlm-roberta-base-ner-hrl`](https://huggingface.co/Davlan/xlm-roberta-base-ner-hrl)
was fine-tuned on modern NER datasets in ten high-resource languages, including
German but not Latin. It serves as a modern multilingual baseline.

Labels in this project: `PER`, `LOC`, `ORG`.

For comparison, `PER` and `PERS` were converted to `PERSON`. Direct comparisons
focus on `PERSON` and `LOC`, the two categories shared by all three models.

## Main Results

| Model | Segments with entities | Coverage | Entity mentions |
|---|---:|---:|---:|
| LatinCy | 7,811 | 82.8% | 17,004 |
| Medieval RoBERTa | 8,015 | 85.0% | 16,172 |
| Multilingual XLM-R | 7,843 | 83.1% | 13,049 |

The models have similar segment coverage but differ substantially in entity
boundaries and label distributions.

Exact PERSON/LOC output overlap was highest between Medieval RoBERTa and
Multilingual XLM-R:

| Model pair | Exact matches | Jaccard similarity |
|---|---:|---:|
| LatinCy × Medieval RoBERTa | 2,670 | 8.8% |
| LatinCy × Multilingual XLM-R | 2,064 | 7.5% |
| Medieval RoBERTa × Multilingual XLM-R | 5,232 | 22.3% |

These values measure output similarity, not accuracy.

## Main Observations

- LatinCy frequently fragments multiword names and normalizes original forms,
  including variation involving I/J and U/V.
- Multilingual XLM-R often produces coherent multiword name boundaries in the
  selected cases, despite not being fine-tuned on Latin NER data.
- A larger number of predicted entities does not necessarily indicate better
  results.
- Person and place boundaries are difficult to define in expressions such as
  *Petri de Leyden*. A flat annotation may treat the complete expression as
  `PERSON`, while nested annotation could also mark *Leyden* as `LOC`.
- Processing heads and sublemmata separately preserves the XML structure but
  removes contextual connections within a lemma.

## Repository Structure

```text
RG1-NER-exploration/
├── notebooks/
│   ├── 01_parse_xml.ipynb
│   ├── 02_data_explore_and_ner.ipynb
│   └── 03_model_comparison.ipynb
├── figures/
├── index.html
├── style.css
├── LICENSE
└── README.md