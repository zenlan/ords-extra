# ORDS Extra

Additional data for the 2025-07 ORDS dataset from the Open Repair Alliance.

NOTE: accuracy is not guaranteed!

## Licence

Made available under the [CC BY-NC-ND 4.0 licence](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.en) © Monique Szpak (https://github.com/zenlan/ords-extra).

## Open Repair Data

The data files in this repo are designed to join to the 2025-07 Open Repair Data aggregate from the [Open Repair Alliance](https://openrepair.org/)

You can get the ORDS data in a [zipfile](https://openrepair.org/open-data/downloads/) or from [Github](https://github.com/openrepair/)

## Files

* `ords_problem_en_deepl.csv` ISO language code and english translation of `problem` text using the DeepL API.
* `ords_problem_en_gemma.csv` ISO language code and english translation of `problem` text using "gemmatranslate" locally, a model based on Gemma3.
* `ords_problem_en_opusmt.csv` ISO language code and english translation of `problem` text using Opus-MT models from the Helsinki-NLP project.
* `ords_product_en.csv` ISO language code and english translation of `partner_product_category` values using 'gemmatranslate' locally, a model based on Gemma3.
* `ords_unu_keys.csv` United Nations University (Unitar) categorisation.

The translation files contain rows for non-English `problem` or `product` strings only.

## Translations

Repair data arrives once a year from all over the world and is hardly grammatically well-formed, often scribbled in the midst of a busy, noisy, messy community repair event. It can contain spelling mistakes, abbreviations, colloquialisms, acronyms, jargon, emojis, brand names, markup, markdown, html and non-printing characters. A lot of data cleaning is carried out prior to publication.

### Language detection

 The datasets arrive with no indication of the language used in each record. At last count there are at least 10 languages represented. The majority of records use one of English, German, Dutch, or French, but there is also some Italian, Spanish, Danish, Norwegian, Chinese, Korean and Welsh to be found.

Over time, I have tried a variety of tools and methods to determine the language and translate the `problem` text. Google detect is very poor with short strings and often gets Danish/Dutch/German confused, DeepL somewhat better but not free.

I trained a small language detection model on repair data and it is fairly successful. Manual tweaking is nonetheless required and the language map will still contain some erroneous detections.

The `product` value is not always in the same language as the `problem` text and therefore requires it's own language detection. TranslateGemma scored marginally better at detection and translation for these short strings than some of the others, although there are still some wild misses.

### Translation Tools

#### Problem Text

* [DeepL API](https://www.deepl.com/en/translator) is probably the best of the three. Am relying on the free monthly allowance so DeepL translations are not yet complete.

* [TranslateGemma](https://ollama.com/library/translategemma), an open translation model built on Gemma 3 has a stab at summarising what it thinks the text might say. Sometimes it produces the opposite meaning.

* [Helsinki-NLP Opus-MT](https://huggingface.co/Helsinki-NLP) can produce some very weird results and likes to inject the ocassional profanity. Does not have a Norwegian-English model.  The Opus MT models are trained using [MarianNMT](https://marian-nmt.github.io/), the same engine used by [Mozilla](https://mozilla.github.io/translations/docs/) for the Firefox translator.

#### Products

ORDS data contains a column that lists the categoory and/or type of product as provided by the contributor. Not all of the partner networks use a standardised set of categories to describe the item brought in for repair, often it is free text. Even when a list is used, miscategorisation is common. The ORDS data maps these product types and categories to it's own standard category set as best it can. See below, "UNU Keys" for an alternative and widely recognised category set.

## UNU Keys

Mapping of the ORDS records to the "UNU Key" categories produced by [UNITAR - United Nations Institute for Training and Research](https://www.unitar.org/).

These keys are commonly found in academic papers dealing with  subjects around e-waste and trickle out to wider reports and news stories, e.g. [OF 16 BILLION MOBILE PHONES POSSESSED WORLDWIDE, 5.3 BILLION WILL BECOME WASTE IN 2022](https://www.unitar.org/about/news-stories/news/16-billion-mobile-phones-possessed-worldwide-53-billion-will-become-waste-2022).

Should the UNU keys change, the mapping in the UNU keys file will be updated.

### Mapping categories to keys

The `product` is split out from the ORDS `partner_product_category` value and searched for regex patterns that map to 'products' that can map to UNU keys.

The result is an improvement on the coarse one-to-one mapping of ORDS `product_category` to UNU key, bearing in mind that:

* Sometimes the categorisation at source was not optimal and/or could have been mapped to the wrong ORDS `product_category`, this can affect the assignment of the UNU key.

* Sometimes a mapping is not obvious from the `product` value, which could be empty, it could be that the `problem` text enabled the categorisation, e.g. `product`="steamer" could be anything, but the `problem` text may have indicated a "steam mop".

* The matching process is not perfect, it has to work across at least 10 European languages and a couple of Asian languages, international characters, spelling mistakes, abbreviations, acronyms, jargon and brand names.

* Manual revision is required to fine-tune the mappings but, given the volume, it is inevitable that some slip through the net

* Work is in progress to translate all `product` values, this might make it easier to formulate regexes but would require excellent translations, which can be tricky to achieve on short strings full of typos, abbreviations etc.

* When a `product` does not find a regex match, it is assigned a default UNU key for its ORDS `product_category`.
