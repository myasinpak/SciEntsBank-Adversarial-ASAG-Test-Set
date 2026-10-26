# Source attribution and changes

## Original source

**SciEntsBank**, part of **SemEval-2013 Task 7: The Joint Student Response Analysis and 8th Recognizing Textual Entailment Challenge**.

Official public repository and license information:
https://github.com/myrosia/semeval-2013-task7

License as stated by the source repository: **Creative Commons Attribution-ShareAlike 3.0 Unported (CC BY-SA 3.0)**.
https://creativecommons.org/licenses/by-sa/3.0/

### Academic citation

Dzikovska, M. O., Nielsen, R., Brew, C., Leacock, C., Giampiccolo, D., Bentivogli, L., Clark, P., Dagan, I., & Dang, H. T. (2013). SemEval-2013 Task 7: The Joint Student Response Analysis and 8th Recognizing Textual Entailment Challenge. *Proceedings of the Seventh International Workshop on Semantic Evaluation (SemEval 2013)*, 263–274. https://aclanthology.org/S13-2045/

## How this dataset differs from the source

This release takes **questions and reference answers** associated with the SciEntsBank Unseen Answers test set, and adds **LLM-generated, human-validated incorrect adversarial answers**. It does **not** reproduce original student responses from the SciEntsBank dataset. The released questions have local IDs (`Q1`, `Q2`, ...), and the adversarial answers have new IDs (`A0001`, ...). The generated answer collection was subjected to quality control, semantic deduplication, independent human evaluation, and question-level balancing.

Credit for the original questions and reference answers belongs to the SciEntsBank / SemEval sources; credit for the synthetic adversarial annotations and curation belongs to the new dataset's authors (complete author metadata before publication). The original SciEntsBank authors were not involved in the development or validation of this adversarial dataset. Their work is acknowledged solely as the source of the original questions and reference answers.
