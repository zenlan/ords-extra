# ORDS Extra

Additional data for the 2025-07 ORDS dataset from the Open Repair Alliance.

New columns with values resulting from a combination of translation tools, regex matching scripts and a little machine learning.

NOTE: accuracy is not guaranteed!

## Licence

Made available under the [CC BY-NC-ND 4.0 licence](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.en) © Monique Szpak (https://github.com/zenlan/ords-extra).

## Open Repair Data

The data files in this repo are designed to join to the 2025-07 Open Repair Data aggregate from the [Open Repair Alliance](https://openrepair.org/)

You can get the ORDS data in a [zipfile](https://openrepair.org/open-data/downloads/) or from [Github](https://github.com/openrepair/)

## Files

* `ords_problem_en.csv` English translation and 2 char ISO language code for each `problem` value.
* `ords_product_en.csv` English translation and 2 char ISO language code for each derived `product` value.
* `ords_unu_keys.csv` United Nations University (Unitar) categorisation.

## Translations

Repair data arrives from all over the world without any indication of the language used in each record. At last count there are at least 10 languages represented. The majority of records use one of English, German, Dutch, French or Danish, but there is also some Italian, Spanish, Chinese, Korean and Welsh to be found.

Repair data is hardly grammatically well-formed, it can contain international characters, spelling mistakes, abbreviations, acronyms, jargon, emojis and brand names.

### Language detection

Over time, I have tried a variety of tools and methods to determine the language and translate the `problem` text. Google detect/translate is adequate to a point, DeepL somewhat better but not free, while Helsinki NLP doesn't detect, can produce iffy translations and does not have all of the language models required. 

Using a combination of the above tools, I eventually developed a language detection model that worked better with the repair data `problem` text. Manual tweaking was nonetheless required and the language map will, no doubt, still contain some erroneous detections. 

As of 2026, I have found that the [Gemma3 AI model](https://ollama.com/library/translategemma) can handle all of the required languages and sometimes produces translations comparable to that of DeepL. Other times it has a stab at summarising what it thinks the text might say and comes to some incorrect - and some amusing - conclusions. The `ords_problem_en.csv` file contains columns for some translations by DeepL, Helsinki-NLP and Google. The Gemma3 column contains a translation attempt for each row.

The `product` value is not necessarily in the same language as the `problem` text and therefore requires it's own language detection. Gemma3 scored marginally better than the other translation tools tried, although there are still some wild misses.

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
