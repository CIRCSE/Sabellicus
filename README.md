# Overview

This repository contains the text of *De Latinae Linguae Reparatione* authored by Marcus Antonius Sabellicus (1436–1506), annotated with respect to lemmas, part-of-speech tags, morphological features and syntactic dependencies.

A first version of the text has been included in the test data of EvaLatin Shared Task (Sprugnoli et al., 2022), which however did not focus on syntactic dependencies. Since that first version, some changes have been implemented - see below ***Annotation formalism and choices*** for further details.

**Acknowledgments**

Annotator: Federica Gamba\
Editor: Flavio Massimiliano Cecchini

We thank Timo Korkiakangas (Helsingin yliopisto, Helsinki, Finland) for funding the annotation through the Suomen Akatemia (Research Council of Finland) project grant no. 315176, *Digital philology and Latin text production: a multimodal analysis of writing in the past*.



# Sources

The raw text was originally downloaded from [ALIM - Archivio della Latinità Italiana del Medioevo](http://alim.unisi.it/dl/resource/484) as a `txt` file, and is reproduced in this repository.
The text in ALIM is based on the critical edition by G. Bottari (1999).

The text is composed of four sections:

- *Epistola a Marcantonio Morosini*
- *De Latinae Linguae Reparatione Marci Antonii Sabellici Dialogus, Qui Et Latinae Linguae Reparatio Inscribitur*
- *Baptistae Guarini Dissertatio*
- *Epistola ad Antonio Moretto*


# Annotation formalism and choices

The document is annotated according to the typological formalism of [Universal Dependencies](https://universaldependencies.org) (UD), in particular as it is applied for Latin. It follows the annotation's state of the art as represented especially by the UDante treebank (Cecchini et al., 2020) and the harmonisation effort by Gamba & Zeman (2023a, 2023b), however implementing some slight (and compatible) twists. These are highlighted in the following, along more general annotation choices which might be of interest to the user. 

## Lexicon

* Lemmas are normalised: they are always lowercase and the couple *v*/*u* is neutralised in favour of *u*.
* The annotation follows the current UD praxis of assigning "adverbs" (`ADV`) their own adverbial lemma. 
    * This means that we observe some etymologically related couples treated as different words (e.g. *facile*/*faciliter*, *nimis*/*nimium*,...), or also, to keep compatibility with other Latin treebanks, elements possibly belonging other parts of speech not annotated as such (e.g. `PRON` for *ubi*, `DET` for *eo*, `NOUN` for *sponte*).
* The current convention of other Latin treebanks is followed in mostly not splitting (i.e. treating as multi-word tokens) compounds of any kind (orthograhical or morphological).
    * This means for example that tokens like *admodum* or *praeterquam* are analysed as unitary elements. The only exception to this is the token *rempublicam*, consisting of two morphologically independent words. Clitics such as *que* and *ue* are instead regularly split (also see below ***Statistics - Words***).
* A complete annotation for foreign words is implemented, choosing the code-switching strategy as detailed in [UD guide lines](https://universaldependencies.org/foreign.html#option-1-code-switched-analysis). Lemmas are represented as in their original language, including possibly different writing systems. On the other hand, the feature `OrigLang` for integrated foreign words has not yet been implemented.
    * We note that while this CoNLL-U overall passes the [official UD validation process](https://universaldependencies.org/validation-rules.html), some morphological features used for `Foreign` words are not (yet) recognised by their respective treebanks (e.g. `InflClass` for `grc`). 

## Morphology

* The lexical features [`NameType`](https://universaldependencies.org/u/feat/NameType.html), [`NumValue`](https://universaldependencies.org/la/feat/NumValue.html) and [`Proper`](https://universaldependencies.org/la/feat/Proper.html) are moved from the features to the miscellaneous (`MISC`) field. 
    * The motivation with regard to `NameType` and `Proper` is that these features are of purely semantic type, with no reflexes whatsoever on morphosyntax, and they actually represent a different, independent layer of annotation (which will need to be better formalised in future), i.e. the one for named entities and multi-word expressions. 
    * As for `NumValue`, the main reason is technical, as its values correspond to the infinite set of natural numbers, and so cannot be listed exhaustively in a UD documentation page.
* `NumValue` is added to all words which are assigned a `NumType`.
* The tripartite system of `VerbForm`s as described in (Cecchini, 2021) is used.
    * This excludes the values `Gdv`, `Ger` and `Sup`. The value `Inf` for infinitives instead of a typologically more transparent `Vnoun` is maintained for compatibility with the other Latin treebanks (we notice that they are in fact equivalent notations for the same object).
    * The `MISC` field stores traditional denominations for Latin tenses and moods by means of the fields [`TraditionalMood`](https://universaldependencies.org/misc.html#traditionalmood) and [`TraditionalTense`](https://universaldependencies.org/misc.html#traditionaltense).
* The Latin Perfect tense (corresponding to forms with *Perfectum* stems) is analysed as having `Aspect=Perf` and `Tense=Pres`. This implies that *Plusquamperfectum* is represented by `Aspect=Perf` and `Tense=Past` (as opposed to *Imperfectum* `Aspect=Imp` and `Tense=Past`), and so `Tense=Pqp` is not used.
* So-called "Inchoative" verbs (i.e. *sc*-verbs) are always annotated with `Aspect=Inch` in their imperfective forms. This is also valid for lemmas which are sometimes considered to "have lexicalised", e.g. *cresco*.
* The features `VerbForm` and `PronType` are also used in an "etymological" sense: that is, they can be assigned to words which are not tagged as `VERB` or `PRON`/`DET`/`ADV` respectively, meaning that the original form was one, even if their current distribution belongs to another part of speech now. 

## Syntax

* The semantic transversal subtypes `lmod` and `tmod` for space and time arguments are applied consistently through the annotation, and so are the corresponding values for `AdvType`.
* So-called "free relatives", or also "double pronouns", are annotated as clauses containing their relative element (e.g. *quando* or *quis*) and as depending on the root of their main clause with the needed clausal relation type (`advcl`, `ccomp`/`xcomp`, `csubj`), using the `relcl` subtype for distinction.
    * This is in contrast to another style of annotation where the relative element is made dependent as a nominal argument of the main clause, and the rest of the free relative depends on the relative element as a "regular" relative clause (`acl:relcl`).



# Statistics

## Words

The annotated texts consists of (for the definitions of *tokens*, *syntactic words* and *multi-word tokens*, please refer to [UD documentation](https://universaldependencies.org/workgroups/newdoc/word_segmentation.html)):

* 246 sentences
* 10651 tokens, or 9070 without considering the 1581 punctuation marks (i.e. part of speech `PUNCT`)
* 10755 syntactic words, or 9174 without considering the 1581 punctuation marks (i.e. part of speech `PUNCT`)

The difference is given by 104 multi-word tokens which are always composed by 2 elements, the second of which is nearly always a functional clitic, distributed as follows:

- 94 *que* (`CCONJ`)
- 3 cum (`ADP`)
- 2 *ue* (`CCONJ`)
- 2 *quis* (`PRON`)
- 1 *quidem* (`PART`)
- 1 *uero* (`ADV`)

The only case where a token is split into two lexical components is *rempublicam*, made out of forms of *res* (`NOUN`) and *publicus* (`ADJ`) respectively.

There are 1866 different lemmas, 1857 if punctuation marks are not included. This leads to a lexical richness (i.e. "type-token ratio") of ca 20,24%. Please note that some words might be different even if they are assigned the same lemma, but then e.g. they belong to different parts of speech.

There are 13 foreign words (marked with `Foreign=Yes`) in the text: 8 belong to Italian (`it`), 5 to Ancient Greek (`grc`). Especially the latter represent cases of code-switching.


## Parts of speech

All UD's parts of speech apart from `SYM` and `X` are used. They are distributed as follows according to their form types and lemmas:


| Part of speech | per form type   | per lemma  |
|------|---|---|
|   `NOUN` + `PROPN`  | 1572 + 404 | 454 + 221  |
|  `VERB`    |  1691 | 508  |
|  `ADJ`    | 862  | 344  |
|  `ADV`   | 845  | 208  |
|   `DET` + `NUM`   | 863 + 25  | 45 + 8  |
|   `ADP`   |  608 |  29 |
|    `PRON`  | 837  | 19 |
|   `SCONJ`   | 366  | 19  |
|   `PART`   |  276 | 16  |
|  `CCONJ`   | 560  | 13  |
|   `AUX`   | 264  | 1  |
| `INTJ`    |  1 |  1 |
|  `PUNCT`    | 1581  | 9  |

Please refer to UD's [guidelines for parts of speech](https://universaldependencies.org/u/pos/index.html). 


## Dependency relations

A total of 61 dependency relations is used, 31 of which represent subtypes of universal relations, as follows:

- `acl`
    - `acl:relcl`
- `advcl`
    - `advcl:abs`
    - `advcl:cmp`
    - `advcl:pred`
    - `advcl:relcl`
- `advmod`
    - `advmod:emph`
    - `advmod:lmod`
    - `advmod:neg`
    - `advmod:tmod`
- `amod`
- `appos`
- `aux`
    - `aux:pass` 
- `case`
- `cc`
- `ccomp`
- `conj`
    - `conj:expl`
- `csubj`
    - `csubj:cleft`
    - `csubj:pass`
    - `csubj:relcl`
- `det`
    - `det:numgov`
- `discourse`
- *`dislocated`* *(NB: never used without subtype)*
    - `dislocated:csubj`
    - `dislocated:nsubj`
    - `dislocated:obj`
- `flat`
    - `flat:gov`
    - `flat:name` 
- `mark`
- `nmod`
- `nsubj`
    - `nsubj:cleft`
    - `nsubj:outer`
    - `nsubj:pass`
- `nummod`
- `obj`
- `obl`
    - `obl:agent`
    - `obl:arg`
    - `obl:cmp`
    - `obl:lmod`
    - `obl:tmod` 
- `orphan`
- *`parataxis`* *(NB: never used without subtype)*
    - `parataxis:rep`
    - `parataxis:reporting`
    - `parataxis:speaker`
- `punct`
- `root`
- `vocative`
- `xcomp`

Please refer to UD's [guidelines for dependency relations](https://universaldependencies.org/u/dep/index.html) and to the [specific guide lines for Latin](https://universaldependencies.org/ext-dep-index.html), where present. 


## Features

The following 24 morpholexical features and their values (given as pipe-separated strings after the equal sign) are used:


- `Abbr=Yes`
- `AdvType=Loc|Tim`
- `Aspect=Imp|Inch|Perf|Prosp`
- `Case=Abl|Acc|Dat|Gen|Loc|Nom|Voc`
- `Compound=Yes`
- `Degree=Abs|Cmp|Dim`
- `Foreign=Yes`
- `Form=Emp`
- `Gender=Fem|Masc|Neut`
- `InflClass=IndEurA|IndEurE|IndEurI|IndEurO|IndEurU|IndEurX|LatA|LatAnom|LatE|LatI|LatI2|LatPron|LatX`
    - also as layer `[nominal]`
- `Mood=Imp|Ind|Sub`
- `Number=Plur|Sing`
    - also as layer `[psor]`
- `NumForm=Roman|Word`
- `NumType=Card|Mult|Ord`
- `PartType=Int`
- `Person=1|2|3`
    - also as layer `[psor]`
- `Polarity=Neg`
- `Poss=Yes`
- `PronType=Con|Dem|Ind|Int|Neg|Prs|Rel|Tot`
- `Reflex=Yes`
- `Tense=Fut|Past|Pres`
- `Variant=Greek`
- `VerbForm=Conv|Fin|Inf|Part`
- `Voice=Act|Pass`



Please refer to UD's [guidelines for morpholexical features](https://universaldependencies.org/u/feat/index.html), [layered features](https://universaldependencies.org/u/overview/feat-layers.html), and to the [specific guide lines for Latin](https://universaldependencies.org/ext-feat-index.html), where present. 




# Contacts

- Federica Gamba, ÚFAL, MFF, Univerzita Karlova, Prague, Czech Republic: `gamba at ufal.mff.cuni.cz`
- Flavio Massimiliano Cecchini, KU Leuven, Belgium (formerly CIRCSE, Universita Cattolica del Sacro Cuore, Milan, Italy): `flavio.cecchini at live.it`

# References

* Federica Gamba and Daniel Zeman. 2023a. [Universalising Latin Universal Dependencies: a harmonisation of Latin treebanks in UD](https://aclanthology.org/2023.udw-1.2/). In *Proceedings of the Sixth Workshop on Universal Dependencies (UDW, GURT/SyntaxFest 2023)*, Washington, DC, USA, March. Association for Computational Linguistics (ACL). 

* Federica Gamba and Daniel Zeman (2023b). [Latin Morphology through the Centuries: Ensuring Consistency for Better Language Processing](https://aclanthology.org/2023.alp-1.7). In *Proceedings of the Ancient Language Processing Workshop associated with the 14th International Conference on Recent Advances in Natural Language Processing RANLP 2023*, Varna, Bulgaria, September.

* Flavio Massimiliano Cecchini, Rachele Sprugnoli, Giovanni Moretti, Marco Passarotti. 2020. [UDante: First Steps Towards the Universal Dependencies Treebank of Dante's Latin Works](https://ceur-ws.org/Vol-2769/paper_14.pdf). In: Johanna Monti, Felice Dell'Orletta, Fabio Tamburini (eds.), *Proceedings of the Seventh Italian Conference on Computational Linguistics*. CEUR Workshop Proceedings, pp. 1-7.

* Flavio Massimiliano Cecchini. 2021. [Formae reformandae: for a reorganisation of verb form annotation in Universal Dependencies illustrated by the specific case of Latin](https://aclanthology.org/2021.udw-1.1). In *Proceedings of the Fifth Workshop on Universal Dependencies (UDW, SyntaxFest 2021)*, pages 1–15, Sofia, Bulgaria. Association for Computational Linguistics.

* Rachele Sprugnoli, Marco Passarotti, Flavio Massimiliano Cecchini, Margherita Fantoli, and Giovanni Moretti. 2022. [Overview of the EvaLatin 2022 Evaluation Campaign](https://aclanthology.org/2022.lt4hala-1.29). In *Proceedings of the Second Workshop on Language Technologies for Historical and Ancient Languages*, pages 183–188, Marseille, France. European Language Resources Association.

* Marco Antonio Sabellico, *De Latinae Linguae Reparatione*, edited by G. Bottari, Messina, Italy. Università degli studi di Messina, Centro interdipartimentale di studi umanistici, 1999 (Percorsi dei classici, 2).


