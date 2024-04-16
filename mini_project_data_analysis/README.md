# Data Analysis Using CMU Movie Corpus

## Introduction

<p align="justify">
In this mini-project, you are given a dataset. Your task is to come up with a set of data analysis questions and answer them using the dataset. We provide a list of suggestions for questions to answer, but you are encouraged to come up with your own questions. You are also encouraged to use additional datasets to enrich your analysis.

The good thing about this data is that its analysis lends itself to different levels of complexity - from relatively simple to very sophisticated. You can choose to limit yourself to applying the concepts you've already learned in Programming, Statistics, and Pattern Recognition. You can also choose to explore new concepts that you haven't learned yet. The choice is yours. 

You are expected to work on this individually and independently.
</p>

## Dataset

|   | CMU Movie Summary Corpus |
|---         |---    |
|**Source**  | http://www.cs.cmu.edu/~ark/personas/	|
|**Tags**    | Text + Graphs + Numerical Data	|
|**Size**    | 46 MB (compressed)	|
|**Typology**| Movie metadata, character metadata and plot summaries, all provided as TXT files.	|
|**Description**| This dataset contains 42,306 movie plot summaries extracted from Wikipedia + aligned metadata extracted from Freebase.	|
|**Relevant Paper**| Learning Latent Personas of Film Characters

In addition, the following datasets may be helpful but it's not necessary to use them:

- [IMDb Datasets](https://developer.imdb.com/non-commercial-datasets/) (title.basics)
- [Oscars](https://www.kaggle.com/datasets/unanimad/the-oscar-award)
- [Freebase](https://developers.google.com/freebase)
- [Tropes](https://github.com/dhruvilgala/tvtropes)

## Suggestions for Milestones 

**1. Understanding Data**

<p align="justify">

- Familiarize yourself with the paper associated with this dataset. It's not necessary to fully understand the technical details of the methods used, but it's important to grasp the dataset's nature and its derivation process.

- Familiarize yourself with the chosen dataset. The best way to do this is by playing around with it, for example, by extracting summary statistics and going through different small samples of the dataset.

- Once you have explored the dataset, decide at least three questions that you want to answer from this dataset. These questions should be interesting to you. 

</p>

**2. Data Handling, Exploratory Analysis, Preprocessing**

The goal of this milestone is to create data handling pipeline, preprocess data, and complete all the necessary descriptive statistics tasks.

When describing the relevant aspects of the data, and any other datasets you may intend to use, you should in particular show (non-exhaustive list):

- That you can handle the data in its size.
- That you understand what’s in the data (formats, distributions, missing values, correlations, etc.).
- That you considered ways to enrich, filter, transform the data according to your needs.
- That you have a reasonable plan and ideas for methods you’re going to use, giving their essential mathematical details in the notebook.

**3. Data Analysis**

<p align="justify">
The goal of this milestone is to answer the questions you have set for yourself. This might involve building a simple classifier or regression model. You may also choose to use more advanced techniques such as natural language processing.
</p>

## Suggestions for Questions to Answer

1. **Decoding the Patterns in Actors' Careers**

-   What characterizes the career trajectories of actors?
-   How do actors navigate and evolve across various genres throughout their careers?
-   Are there observable patterns in the network connecting actors?
-   How have gender and age shaped the landscape of recognition and success in the history of the Oscars?

2. **The User-Critic Divide in Cinema**

-    Do users and critics have different tastes?
-    If yes, how does each of these features impact the difference independently?
-    Finally, can we find global features explaining that discrepancy and how well?

## Deliverables


<p align="justify">

-    A `README.md` file containing:
        - Title
        - Abstract: A 150 word description of the project idea and goals. What’s the motivation behind your project?
        - Data Analysis Questions: A list of data analysis questions you would like to address during the project.
        - Additional datasets (if any): List the additional dataset(s) you use. 
        - Methods and Results

</p>

<p align="justify">

- Notebook containing data handling pipelines, preprocessing steps and data analysis. We will grade the correctness, quality of code, and quality of textual descriptions. There should be a single Jupyter notebook containing the main logic. The implementation of helper functions that is not essential for understanding the main logic should be contained in external scripts/modules that will be called from the main notebook.
</p>

## Grading

<p align="justify">
We're going to evaluate your progress during the weeks of 23/25 April and 7/9 May. We'll ask you to walk us through your Jupyter notebook (`README.md` not needed at this stage). 

The deadline for submission of notebook and `README.md` on LMS portal is 10 May. 
</p>

