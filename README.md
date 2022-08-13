# SkillSpan Repository
This repository contains the code and data for the paper:

__SkillSpan: Hard and Soft Skill Extraction from Job Postings__

Mike Zhang, Kristian Nørgaard Jensen, Sif Dam Sonniks, and Barbara Plank. To appear at the 2022 Annual Conference of the North American Chapter of the Association for Computational Linguistics (NAACL). 2022

If you use the code, data, guidelines, models from SkillSpan, please include the following reference:


```
@inproceedings{zhang-etal-2022-skillspan,
    title = "{S}kill{S}pan: Hard and Soft Skill Extraction from {E}nglish Job Postings",
    author = "Zhang, Mike  and
      Jensen, Kristian N{\o}rgaard  and
      Sonniks, Sif  and
      Plank, Barbara",
    booktitle = "Proceedings of the 2022 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies",
    month = jul,
    year = "2022",
    address = "Seattle, United States",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2022.naacl-main.366",
    pages = "4962--4984",
    abstract = "Skill Extraction (SE) is an important and widely-studied task useful to gain insights into labor market dynamics. However, there is a lacuna of datasets and annotation guidelines; available datasets are few and contain crowd-sourced labels on the span-level or labels from a predefined skill inventory. To address this gap, we introduce SKILLSPAN, a novel SE dataset consisting of 14.5K sentences and over 12.5K annotated spans. We release its respective guidelines created over three different sources annotated for hard and soft skills by domain experts. We introduce a BERT baseline (Devlin et al., 2019). To improve upon this baseline, we experiment with language models that are optimized for long spans (Joshi et al., 2020; Beltagy et al., 2020), continuous pre-training on the job posting domain (Han and Eisenstein, 2019; Gururangan et al., 2020), and multi-task learning (Caruana, 1997). Our results show that the domain-adapted models significantly outperform their non-adapted counterparts, and single-task outperforms multi-task learning.",
}
```

## Models

All models used in this paper can be found at: https://huggingface.co/jjzha

## Cloning this repo

This repo contains a submodule which contains the source code for MaChAmp (mtp). 
To clone both repos (this + the submodule) use the following command:

```bash
$ git clone --recurse-submodules https://github/kris927b/SkillSpan.git
```

Or use these two commands:

```bash
$ git clone https://github/kris927b/SkillSpan.git 
$ git submodule update --init --recursive
```

## Data set acquisition

As pointed out in the paper, we release two of the three subsets. The subsets are de-identified according to GDPR regulations:

__[20/04/2022]__: Currently, subset 1 (Tech) is de-identified. We are in the process of de-identifying subset 2 (House).

__[03/06/2022]__: All available subsets are de-identified. Find them below.

__[06/07/2022]__: `.conll` files were weirdly formatted. Changed it.

Subset 1 (Tech): https://drive.google.com/file/d/1Xdx6CuCXZyQ9etqJphXk5vvkTu4wk5jm/view?usp=sharing

Subset 2 (House): https://drive.google.com/file/d/1BDVicve-3ug-PuOTnO-Bx4l4-6Ih6r9U/view?usp=sharing

*Note:* The data can also be found now in `data/Skills/*`.

The data is structured in the `conll` format:

```
Token <\t> Skill-tag <\t> Knowledge-tag

e.g.,
Python <\t> O <\t> B-Knowledge
...
```

Place all the data in `data/Skills`

### Licensing

This work is licensed under a Creative Commons Attribution 4.0 International License.

## Running the code

### Installing the requirements

To install all the required packagse run the following command

```
pip3 install --user -r machamp/requirements.txt
```

### Running everything

Once you have installed the required packages, you are now ready to run the experiments. 

We have provided three different scripts for running the experiments. 

To run everything in the paper, simply call:

```
bash scripts/run.all.sh
```

### Running individual experiments

If you rather than running all experiments wants to run individual experiments, we have provided two scripts. 

One for running an experiment as in JobBERT on Skills where it runs all 5 seeds:

```
MODEL = bert | jobbert | spanbert | jobspanbert
EXPERIMENT = skills | knowledge | multi

bash scripts/run.experiment.sh $MODEL $EXPERIMENT
```

And another one for running individual seed numbers within an experiment. So running JobBERT on Skills seed 1:

```
MODEL = bert | jobbert | spanbert | jobspanbert
EXPERIMENT = skills | knowledge | multi
SEED = 1-5

bash scripts/run.individual.sh $MODEL $EXPERIMENT $SEED
```

### Predicting on dev and test set

To predict on the dev and test sets we have provided the following script for convenience

```
MODEL = bert | jobbert | spanbert | jobspanbert
TYPE = skills | knowledge | multi
SET = dev | test

bash scripts/run.predict.sh $MODEL $TYPE $SET
```

### Evaluate the performance

To evaluate the models performance run 

```
bash scripts/run.evaluate.sh
```

This will generate a bunch of metric files in which you can find the results.

#### Run ASO 

To create the ASO stats for the models run

```
bash scripts/run.significance.sh
```

This will generate the ASO scores for all experiments present in the metrics folder produced by the evaluate script. 
