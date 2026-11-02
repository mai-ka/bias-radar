# BiasRadar

A free tool that checks German-language text collections for bias before AI
training, runs locally, and backs its findings with evidence passages.

**Status: concept stage.** There is no working code yet. This repository
documents the idea, the planned approach and how it relates to existing tools.

## The problem

Language models learn from text and inherit its biases. This starts in the
training data, not in the finished model. Teams that fine-tune models or build
their own corpora rarely have a fairness department, and there is hardly any
accessible tool that checks raw text collections beforehand, especially for
German.

## The approach

- **Local:** your data never leaves your machine.
- **Several established methods in one run:** named entity recognition,
  co-occurrence and association measures with confidence intervals. We start
  with gender and plan to extend to further dimensions such as origin, skin
  colour and religion.
- **Built for German:** grammatical gender, generic masculine and
  gender-inclusive forms are taken into account.
- **A report you can check:** HTML, PDF and JSON, with evidence passages and
  reproducible settings (configuration, word list version, corpus checksum).
- **Reference to Article 10 of the EU AI Act** (data and data governance).
- **Python library, command line and a simple local web interface**, so people
  without programming skills can use it too.

## Who it is for

Teams that fine-tune language models or prepare their own corpora without a
dedicated fairness team. NGOs, public bodies and equality offices that need to
review partner projects or data sources. People responsible for documenting
training data under the EU AI Act.

## How it differs from existing tools

See [benchmarking.md](benchmarking.md). The individual methods are not new.
What is new is the combination: local, German-aware, one report with evidence.

## Planned roadmap

1. Word lists, reference corpora, evaluation design; core library
2. Association metrics with confidence intervals; interface prototype
3. Report module; interface implementation
4. Evaluation with inserted bias and reference corpora, documentation,
   release 0.1

## Team

- **Maika Paetzold** ([mettage.com](https://mettage.com), [@mai-ka](https://github.com/mai-ka)):
  concept, interface, user testing
- **Fabian Franz** ([@Franzelfx](https://github.com/Franzelfx)):
  technical implementation, architecture, evaluation

## Contributing

Pointers to tools we missed, word lists and bug reports are welcome. Please
open an issue.

## License

To be decided.
