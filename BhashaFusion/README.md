# BhashaFusion (PhoneticMap)
A package used to 
- convert indic language like hinid, kannada, malayalam,telugu,Odia, Bengali,Gujarathi  to `iast` and 
- `iast` to Other inidc langauge(hinid, kannada, malayalam,telugu,Odia, Bengali,Gujarathi) viceversa.
- `Phonetic Word Search`: Searching word which sound similar or Phoneticly Similar in different languages like hinid, kannada, malayalam,telugu,Odia,Bengali,Gujarathi...etc
- `Not Started`: addon for bash regual expression function to include `Phonetic Word Search` for Indic Language 
-  `Inprogress`: Mapping all Indic alphabets to [International Phonetic Alphabet (IPA)](https://en.wikipedia.org/wiki/International_Phonetic_Alphabet) 
- `Not Started`: Generated Phonetic Word  given new indic word using  Phonetic Mapped Alphabet
- `Not Started`: Generating audio file for  indic word
- `Not Started`: Pull request of Postgress to include **Phonetic Alphabet Search** in IPA and IAST format 

### Installation
```bash
pip install BhashaFusion  
```




### Usage
```python
import sqlite3
import os
import sys

from BhashaFusion import PhoneticMap

#create a IAST object
phoneticmap = PhoneticMap()

# customization
# phoneticmap = PhoneticMap(db_path='iast-token.db', table_name_alpha='IndianAlphabet',table_name_barakadi='Barakhadi')
```

### Converting All Indic language(hinid, telugu, kannada, Malayalam, Odia, Bengali&Assamese, Gujarati, tamil) to iast 
**`InProgress`** Research and Analysis is going on in  Tamil Script, Nastaliq Script, Sinhala Script.

```python
phoneticmap.to_iast('''ଧୃତରାଷ୍ଟ୍ର ଉଵାଚ |\tধৃতরাষ্ট্র উবাচ |\tધૃતરાષ્ટ્ર ઉવાચ |\tத்றுதராஷ்ட்ர உவாச |''')
# >>> 
# dhr̥tarāṣṭra uvāca |	dhr̥tarāṣṭra ubāca |	dhr̥tarāṣṭra uvāca |	ta்ṟutarāṣa்ṭa்ra uvāca |
```


### Convert `iast` to Indic Language 
Currently this can convert `IAST` to kannada, hindi, telugu, malyalam 

```python
word = 'kaṁ  itāḥ kiṁ  yuyutsavaḥ kl̥̄ kl̥ pāṇḍavānīkaṁ itāḥ kiṁ āṁ  īṁ   yuyutsuṁ  kiṁ rānsakhīṁstathā'
print(PhoneticMap.iast2tokens( word) )
# >>> ['k', 'a', 'ṁ', '  ', 'i', 't', 'ā', 'ḥ', ' ', 'k', 'i', 'ṁ', '  ', 'y', 'u', 'y', 'u', 't', 's', 'a', 'v', 'aḥ', ' ', 'k', 'l̥̄', ' ', 'k', 'l̥', ' ', 'p', 'ā', 'ṇ', 'ḍ', 'a', 'v', 'ā', 'n', 'ī', 'k', 'a', 'ṁ', ' ', 'i', 't', 'ā', 'ḥ', ' ', 'k', 'i', 'ṁ', ' ', 'ā', 'ṁ', '  ', 'ī', 'ṁ', '   ', 'y', 'u', 'y', 'u', 't', 's', 'u', 'ṁ', '  ', 'k', 'i', 'ṁ', ' ', 'r', 'ā', 'n', 's', 'a', 'kh', 'ī', 'ṁ', 's', 't', 'a', 'th', 'ā']
indic_lang = 'Telugu' # 'Kannada' # 'Telugu', 'Odia', 'Gujarati', 'Bengali-Assamsese', 
# indic_lang='Devanagari'
# indic_lang='Kannada'
# indic_lang='Telugu'
# indic_lang='Odia'
# indic_lang='Bengali–Assamese'
# indic_lang='Tamil' # In development state

print(phoneticmap.iast2indic(word,indic_lang))



PhoneticMap.dict_tokens2indic(dict_tokene_list,halant)
# >>> కం  ఇతాః కిం  యుయుత్సవః  పాణ్డవానీకం ఇతాః కిం ఆం  ఈం  కిం యుయుత్సుం రాన్సఖీంస్తథా
```


### Phonetic Hash for Phonetic Search 
Presently there are two type of Phonetic Hash
- BASIC HASHING :  <br>
    - all vowel are removed  and <br>
    - consonats are truncated <br>
        eg: kh is mapped to k, <br> 
        श, ष, स are mapped to स (s)<br>
        ङ,ञ,ण,न are mapped न (n) <br>
        ..etc


- NORMAL HASHING
    - vowel are truncated and <br>
    - consonats are truncated

```python
zero_vowels={ '':['a', "ā", "â","i", "ī","u", "ū",chr(805),chr(803),
                    "l̥", "l̥̄","e", "ē", "ê","o", "ō", "ô",
                    "ṁ", "m̐", "ṃ", "ṃ","n̆", "n̆", "n̆","ḥ" , "ḫ", "ẖ", "ḥ"],
                'r': ["r̥", "r̥̄"]
            } # replacing with r is not working for 'r̥' so we replace with chr(805) above
truncated_vowels = { '':[chr(805), chr(803), chr(772),chr(784),chr(774)],
                    'a':["ā", "â"], 
                    'i':["i", "ī"], 
                    'u':["u", "ū"], 
                    'r':["r̥", "r̥̄"],
                    'l':["l̥", "l̥̄"],
                    "e":["e", "ē", "ê"],
                            # "ai", 
                    "o": ["o", "ō", "ô"], 
                                            # "au",
                    'm' :["ṁ", "m̐", "ṃ", "ṃ"], 
                    'n': ["n̆", "n̆", "n̆"], 
                    'h' :["ḥ" , "ḫ", "ẖ", "ḥ"],
                    }
basic_truncated_consonat = {
                    'k' : ['ḵ', 'k', 'kh','k͟ha'],
                    'g' :['g','g̈','gh','ġ'],
                    'n' : ['ṅ','n̆','ñ','ṇ','ṉ'],
                    'c' : ['c', 'ĉ','ch','ĉh'],
                    'j' : ['j','ǰ', 'ĵ', 'jh'],
                    't': ['ṭ','ṭa','t','th','ṯ','t̤'],
                    'd' : ['ḍ', 'd̤','ḍ','ḍh','d','dh','ḏ'],
                    'p' : ['p', 'ph'],
                    'b' : [ 'b', 'b̤', 'bh'],
                    'm' : ['m̆' ],
                    'r' : ['ṟ', 'r̆'],
                    'l' :['ḻ', 'ḷ', 'l̤'],
                    'y' : ['y', 'ẏ', 'y̌'],
                    's': ['ś', 'ṣ', 's','s̱', 's̤','sh' ],
                    "z": ["z","ž","ž","ž",'ẕ','ẕ','ẓ','ż'],
                    'h' : ['h','h̤']
                    }
```

```python
search_word = 'dhr̥tarāṣṭra uvāca'
search_word = search_word.strip().lower()



# to_iast
search_iast = phoneticmap.to_iast(search_word) # similar to idempotent matrx no loss of info if ':' not present
# >>> dhr̥tarāṣṭra uvāca
print(search_iast)

print("# Original Text:", search_word)
print('BASIC HASHING: ',PhoneticMap.basic_hash(search_iast))
print('NORMAL HASHING',PhoneticMap.normal_hash(search_iast))

# >>> Original Test: dhr̥tarāṣṭra uvāca
# >>> BASIC HASHING: drtrstr vc
# >>> NORMAL HASHING: drtarastra uvaca

```




### Contribute
**`InProgress`** Research and Analysis is going on in  Tamil Script, Nastaliq Script(Urdu, Arabic), Sinhala(Sri Lanka) Script. <br>
**`Not Strated`** Marathi can easily added into this package


### Issue
Please [open an issue ](https://github.com/dankarthik25/BhashaFusion/issues "open an issue ")<br>
here in case any bug was encountered. 
Mail id : dankarthik25@gmail.com
