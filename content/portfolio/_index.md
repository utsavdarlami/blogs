---
title: Utsav Darlami
description: Portfolio of Utsav Darlami, machine learning engineer.
intro: I build machine learning models and get them into production.
email: utsavdarlami17@gmail.com
github: https://github.com/utsavdarlami
industry:
  role: Machine Learning Engineer
  org: Docsumo
  dates: Sep 2021 – Apr 2025
  summary: Production document AI for enterprise clients. Every model below ran in production.
  stack: Python, PyTorch, Transformers, YOLO, scikit-learn, XGBoost, FastText, spaCy, LiteLLM, Langfuse, Pydantic, FastAPI, GCP
  items:
    - model: LLM extraction package
      text: As lead developer, built the Python package behind the company's automated extraction API, with LiteLLM routing, Pydantic validation, retries, and Langfuse tracing. Benchmarked GPT-4o, Mixtral-8x7B, and Llama 3.1 for table extraction (about 75% accuracy with GPT-4o) across 300+ documents on average for two enterprise clients.
    - model: LayoutLM and variants
      text: Fine-tuned for key-value pair extraction from business documents. Worked on part of the single-GPU training pipeline and ran quantization benchmarks weighing accuracy against inference speed and memory.
    - model: YOLO
      text: Fine-tuned to find the parts of a bank cheque, including MICR lines, amounts, dates, and signatures.
    - model: Random Forest and XGBoost
      text: Worked on the featurization-to-training pipeline that turns OCR tables into features for spotting column labels and valid rows. Cut featurization time by 60%.
    - model: FastText and spaCy
      text: Built and deployed a text classification API on FastAPI and GCP that four downstream services rely on.
sections:
  - name: Agents and LLM tools
    projects:
      - name: CanvasChat
        url: https://github.com/utsavdarlami/CanvasChat
        text: Arranges charts and data on a shared canvas from plain-language commands. A four-stage agent pipeline (extraction, context assembly, intent to plan, layout execution) turns each request into a plan you can review, synced live over WebSockets.
        stack: TypeScript, React, Python, Gemini, Google ADK, WebSockets
      - name: CReview
        url: https://github.com/utsavdarlami/creview
        text: Code review that feeds static analysis linter output to an LLM for context-aware feedback.
        stack: Python, LLMs, linters
      - name: Agent starter template
        url: https://github.com/utsavdarlami/Agent-Starter-Template
        text: A starting point for Google ADK agents with state management and tool calling set up.
        stack: Python, Google ADK
  - name: Vision and deep learning
    projects:
      - name: Sandstone segmentation
        url: https://github.com/utsavdarlami/sandstone_segmentation
        text: Labels each pixel of a sandstone micrograph as quartz, pore, or clay, using Gaussian, Sobel, and Gabor filter features and a Random Forest. 95% F1 (Dice).
        stack: Python, OpenCV, scikit-learn
      - name: Microplastic detection
        url: https://github.com/simonspurs/Microplastic-
        text: YOLO detection of microplastic particles in soil-sample microscope images, behind the co-authored paper in *Microplastics* (2026). I built the training notebook and an inference app with single-image and bulk analysis, nano to medium model sizes, a results database, and a ground-truth versus prediction viewer.
        note: Repository hosted by a co-author.
        stack: Python, YOLO, Streamlit, SQLite
      - name: K-means color quantization
        url: https://github.com/utsavdarlami/KMeansColorQuantization
        text: K-means written from scratch in C++ and used to shrink an image's palette.
        stack: C++
      - name: Nepali license plate recognition
        url: https://github.com/utsavdarlami/NepalLicensePlateRecognition
        text: Finds plates in video with YOLOv2, splits characters with Otsu thresholding, and reads Devanagari characters with a custom CNN at 96% accuracy.
        stack: Python, YOLOv2, Keras, OpenCV
      - name: Genetic algorithm image segmentation
        url: https://github.com/utsavdarlami/GA_For_ImageSegmentation
        text: Segments images by evolving clustering boundaries with a genetic algorithm.
        stack: Python
      - name: Learning PyTorch
        url: https://github.com/utsavdarlami/learning_pytorch
        text: Models written from scratch, including variational, conditional, and adversarial autoencoders on MNIST and CelebA, graph attention networks, and the basics underneath (autograd, perceptron, Adaline).
        stack: Python, PyTorch, PyTorch Geometric
  - name: Classical ML and optimization
    projects:
      - name: Evolutionpy
        url: https://github.com/utsavdarlami/evolutionpy
        text: A packaged Python library of evolutionary optimizers for problems without gradients, designed so custom fitness functions slot in easily.
        stack: Python, NumPy
      - name: BreakfastScoop
        url: https://github.com/utsavdarlami/BreakfastScoop
        text: A Flask app that scrapes live news from Nepali sites and sorts it into 10 topics with Naive Bayes at 81% F1.
        stack: Python, BeautifulSoup, Flask, scikit-learn
  - name: Web
    projects:
      - name: CreoV2 and Django-Creo
        url: https://github.com/utsavdarlami/creoV2
        text: An image, video, and art sharing platform. I built the Django backend API and database schema.
        stack: JavaScript, Django, Python
publications:
  - title: "Microplastic Contamination in High-Altitude Soils of Sagarmatha National Park: A Spatial Assessment with Deep Learning-Supported Detection"
    url: https://doi.org/10.3390/microplastics5030145
    venue: "*Microplastics*, 2026"
    authors: Baniya, S., Mohanta, T., Yacoub, M., **Darlami, U.**, Li, A., Subedi, I., Nicholson, K., Sharma, S., and Han, B.
  - title: "Artificial Neural Network-Genetic Algorithm for Optimization of Multivariate Function: An Application to Lactic Acid Production"
    url: /blogs/misc/slide_ann_ga.pdf
    venue: Talk, National Conference on Mathematics and Its Applications (NCMA), Nepal, June 2022
---
