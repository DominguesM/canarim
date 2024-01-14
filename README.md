
<p align="center">
  <img width="250" alt="Camarim Logo" src="https://raw.githubusercontent.com/DominguesM/Canarim-Instruct-PTBR/main/assets/canarim.png">
</p>

<p align="center">
  <a href="https://huggingface.co/datasets/dominguesm/canarim">[🐱 HuggingFace]</a>
</p>

<hr>


# Canarim: A Large-Scale Dataset of Web Pages in the Portuguese Language

## Introduction

Canarim is a database encompassing over 342 million Portuguese language documents, sourced from multiple iterations of CommonCrawl. This nearly 1 terabyte database stands as one of the most extensive Portuguese language data collections available. It underwent initial deduplication using URLs, with plans for further text-based deduplication and filtering of potentially harmful content. The data, originally in HTML, has been converted to Markdown with the `Trafilatura` library to enhance readability and quality. Canarim is poised to be a crucial resource for NLP research, particularly in Portuguese language applications, filling the gap in large-scale, high-quality data for languages other than English.

## Dataset Structure

### Data Instances

An example looks as follows:

```json
{
  "url": "...",
  "content_languages": "por",
  "warc_filename": "crawl-data/CC-MAIN-2023-06/segments/1674764500041.18/warc/CC-MAIN-20230202200542-20230202230542-00352.warc.gz",
  "warc_record_offset": 971279893,
  "warc_record_length": 3873,
  "text": "...",
  "crawl_timestamp": "2023-02-02T20:28:21Z"
}
```

### Data Fields

- `url`: URL of the page
- `content_languages`: Language of the page
- `warc_filename`: Name of the WARC file
- `warc_record_offset`: Offset of the WARC record
- `warc_record_length`: Length of the WARC record
- `text`: Text of the page, in Markdown format
- `crawl_timestamp`: Timestamp of the crawl

## Text Extraction Overview

The Canarim database employs the [`Trafilatura`](https://trafilatura.readthedocs.io) library for extracting textual content from HTML data, converting it into Markdown format. This tool focuses on preserving key textual elements like titles, subtitles, bold, and italic formatting in Markdown, ensuring the retention of the original document structure. During the extraction process, Trafilatura discards comments and other non-essential information, streamlining the content to include only the main body of the web pages.

</br>

<p align="center">
  <img width="800" alt="Text Extraction Example" src="https://raw.githubusercontent.com/DominguesM/canarim/main/assets/canarim-text-extraction-preview.png">
</p>
<p align="center">
  <a href="https://g1.globo.com/ac/acre/natureza/amazonia/noticia/2023/01/03/para-comemorar-40-anos-do-parque-zoobotanico-da-ufac-livro-vai-reunir-depoimentos-de-envolvidos-no-inicio-do-projeto.ghtml" target="_blank">Original Web Page</a> and 
  <a href="https://github.com/DominguesM/canarim/blob/main/assets/extracted_text.md" target="_blank">Extracted Text</a>
</p>

## Usage

Below is an example of how to quickly explore just a few samples from a dataset using the `datasets` library.

```python
!pip install -q datasets

from datasets import load_dataset

ds = load_dataset(
    "dominguesm/canarim",
    # Filter only the data from the `train split`
    split="train",
    # Filter only the files that contain the prefix `train/data-0019` and the suffix `-of-00192.arrow`
    data_files="train/data-0019*-of-00192.arrow",
    # Load the dataset without downloading the data (Streaming mode)
    streaming=True
)

# From the returned data, filter only the data where the `url` value starts with `https://g1.globo.com/`
ds_globo = ds.filter(
    lambda example: example['url'].startswith("https://g1.globo.com/")
)

# Return the first 10 examples from the applied filter.
data = list(ds_globo.take(10))

print(data[0])

# {
#     "url": "https://g1.globo.com/ac/acre/(...)",
#     "content_languages": "por",
#     "warc_filename": "crawl-data/CC-MAIN-2023-06/segments/1674764499919.70/warc/CC-MAIN-20230201081311-20230201111311-00552.warc.gz",
#     "warc_record_offset": 281625400,
#     "warc_record_length": 192934,
#     "text": "Parque Zoobotânico da Ufac guarda uma grande variedade espécies de árvores em Rio Branco — Foto: Arquivo/Ufac (...)",
#     "crawl_timestamp": "2023-02-01T10:38:52Z"
# }
```

## Dataset Statistics

| Split  | # Samples | # Size (bytes) | # Size (GB) |
| ------ | --------- | -------------- | ----------- |
| Train  | 342,818,651 | 1,087,519,823,221 | 1087,51 |

## Citing

If you use Canarim in your research, please cite the following.
  
```bibtex
  @misc {maicon_domingues_2024,
    author       = { {Maicon Domingues} },
    title        = { canarim (Revision 640e079) },
    year         = 2024,
    url          = { https://huggingface.co/datasets/dominguesm/canarim },
    doi          = { 10.57967/hf/1605 },
    publisher    = { Hugging Face }
  }
```

## License

This dataset is licensed under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). You can use the dataset for any purpose, but you must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.

## Contact

For any questions or suggestions, please contact [Maicon Domingues](https://nlp.rocks/).
