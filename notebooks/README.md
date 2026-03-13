# Notebooks

Notebooks were made in Google Colab with GPU runtimes.

## Running in Colab

1. Open the notebook in Google Colab
2. Set the runtime to GPU: **Runtime → Change runtime type → A100 or G4 or H100 GPU**
3. Upload the required data files to the Colab session storage
4. Run all cells in order

## Notebooks

| Notebook | Colab | Description |
|---|---|---|
| [01_SpeciesBiasPLM.ipynb](https://github.com/chrisnguyen11/protein-language-modeling/blob/main/notebooks/01_SpeciesBiasPLM.ipynb) | [Open in Colab](https://colab.research.google.com/drive/1tGa-lnTZHaIUhloOtYavDlWk6iBAgyLu?usp=sharing) | Comparing *A. baylyi* and *E. coli* log-likelihoods under ESM-2 |
| [02_Finetuning_ESM.ipynb](https://github.com/chrisnguyen11/protein-language-modeling/blob/main/notebooks/02_Finetuning_ESM.ipynb) | [Open in Colab](https://colab.research.google.com/drive/1Qaa92ZdTVAYlBb7HkBJu_bBwnxNWOSk9?usp=sharing) | Fine-tuning ESM-2 on *A. baylyi* |
| [03_ESM_Decoder.ipynb](https://github.com/chrisnguyen11/protein-language-modeling/blob/main/notebooks/03_ESM_Decoder.ipynb) | [Open in Colab](https://colab.research.google.com/drive/1KM5C2OYeP6i8uyMQI3QSqQLee7qx3Lkd?usp=sharing) | Training decoders on ESM-2 embeddings |

## Data

| File | Relevant Notebooks | Description | Source |
|---|---|---|---|
| `proteome-taxid_83333.fasta` | 01, 02 | *E. coli* protein coding | [NCBI Genome](https://www.uniprot.org/proteomes/UP000000625) |
| `proteome-taxid_62977.fasta` | 01, 02 | *A. baylyi* protein coding | NCBI Genome |
| `uniref201803_ur50_valid_headers.txt` | 02, 03 | UniRef50 headers | [ESM Repo](https://github.com/facebookresearch/esm?tab=readme-ov-file#pre-training-dataset-split--) |