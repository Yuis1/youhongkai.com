---
{"dg-publish":true,"permalink":"/CS计算机科学/大模型/RAG/unstructured.io专题/","noteIcon":"","created":"2024-08-28T17:27:38.000+08:00","updated":"2024-04-24T00:05:56.000+08:00"}
---


作者：游鱼思

---
# unstructured.io

## 简介

https://unstructured.io/

The unstructured library includes functions to partition, chunk, clean, and stage raw source documents.

提供了 本地运行、API服务、SAAS服务三种方式。

## Partitioning 分区

PDF处理

The strategy kwarg controls the method that will be used to process the PDF. The available strategies for PDFs are "auto", "hi_res", "ocr_only", and "fast".

The "auto" strategy will choose the partitioning strategy based on document characteristics and the function kwargs. If infer_table_structure is passed, the strategy will be "hi_res" because that is the only strategy that currently extracts tables for PDFs. Otherwise, "auto" will choose "fast" if the PDF text is extractable and "ocr_only" otherwise. "auto" is the default strategy.

The "hi_res" strategy will identify the layout of the document using detectron2[!https://github.com/facebookresearch/detectron2].

The advantage of “hi_res” is that it uses the document layout to gain additional information about document elements. We recommend using this strategy if your use case is highly sensitive to correct classifications for document elements. If detectron2 is not available, the "hi_res" strategy will fall back to the "ocr_only" strategy.

The "ocr_only" strategy runs the document through Tesseract for OCR and then runs the raw text through partition_text. Currently, "hi_res" has difficulty ordering elements for documents with multiple columns. If you have a document with multiple columns that does not have extractable text, we recommend using the "ocr_only" strategy. "ocr_only" falls back to "fast" if Tesseract is not available and the document has extractable text.

The "fast" strategy will extract the text using pdfminer and process the raw text with partition_text. If the PDF text is not extractable, partition_pdf will fall back to "ocr_only". We recommend using the "fast" strategy in most cases where the PDF has extractable text.

To extract images and elements as image blocks from a PDF, it is mandatory to set strategy="hi_res" when setting extract_images_in_pdf=True. With this configuration, detected images are saved in a specified directory or encoded within the file. However, keep in mind that extract_images_in_pdf is being phased out in favor of extract_image_block_types. This option allows you to specify types of images or elements, like “Image” or “Table”. If some extracted images have content clipped, you can adjust the padding by specifying two environment variables “EXTRACT_IMAGE_BLOCK_CROP_HORIZONTAL_PAD” and “EXTRACT_IMAGE_BLOCK_CROP_VERTICAL_PAD” (for example, EXTRACT_IMAGE_BLOCK_CROP_HORIZONTAL_PAD = 20, EXTRACT_IMAGE_BLOCK_CROP_VERTICAL_PAD = 10). For integrating these images directly into web applications or APIs, extract_image_block_to_payload can be used to convert them into base64 format, including details about the image type. Lastly, the extract_image_block_output_dir can be used to specify the filesystem path for saving the extracted images when not embedding them in payloads.

## Cleaning 清理


## Extrating 提取


## Chunking 分块



## Embedding 嵌入


## Llama Hub 的 UnstructuredReader

partition步骤：首选从传入的url、api来运行partition服务，如果没有，则在本地运行。

本地运行依赖较重：paddleocr

Depending on the constructin if url is set or api = True

        it'll parse file using API call, else parse it locally

        extra_info is extended by the returned metadata if 

        split_documents is True
