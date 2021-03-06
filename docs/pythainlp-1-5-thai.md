# คู่มือการใช้งาน PyThaiNLP 1.5

[TOC]

Natural language processing หรือ การประมวลภาษาธรรมชาติ  โมดูล PyThaiNLP เป็นโมดูลที่ถูกพัฒนาขึ้นเพื่อพัฒนาการประมวลภาษาธรรมชาติภาษาไทยในภาษา Python และ**มันฟรี (ตลอดไป) เพื่อคนไทยและชาวโลกทุกคน !**

> เพราะโลกขับเคลื่อนต่อไปด้วยการแบ่งปัน

รองรับ Python 2.7 และ Python 3.4 ขึ้นไปเท่านั้น

ติดตั้งใช้คำสั่ง

```
pip install pythainlp
```

**วิธีติดตั้งสำหรับ Windows**

ให้ทำการติดตั้ง pyicu โดยใช้ไฟล์ .whl จาก [http://www.lfd.uci.edu/~gohlke/pythonlibs/#pyicu](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pyicu) 

หากใช้ python 3.5 64 bit ให้โหลด PyICU‑1.9.7‑cp35‑cp35m‑win_amd64.whl แล้วเปิด cmd ใช้คำสั่ง

```
pip install PyICU‑1.9.7‑cp35‑cp35m‑win_amd64.whl
```

แล้วจึงใช้ 

```
pip install pythainlp
```

**ติดตั้งบน Mac**

```sh
$ brew install icu4c --force
$ brew link --force icu4c
$ CFLAGS=-I/usr/local/opt/icu4c/include LDFLAGS=-L/usr/local/opt/icu4c/lib pip install pythainlp
```

ข้อมูลเพิ่มเติม [คลิกที่นี้](https://medium.com/data-science-cafe/install-polyglot-on-mac-3c90445abc1f#.rdfrorxjx)

## API

### tokenize

#### word_tokenize

สำหรับการตัดคำไทยนั้น ใช้ API ดังต่อไปนี้

```python
from pythainlp.tokenize import word_tokenize
word_tokenize(text,engine)
```
text คือ ข้อความในรูปแบบสตริง str เท่านั้น

engine คือ ระบบตัดคำไทย ปัจจุบันนี้ PyThaiNLP ได้พัฒนามี 6 engine ให้ใช้งานกันดังนี้

1. icu -  engine ตัวดั้งเดิมของ PyThaiNLP (ความแม่นยำต่ำ) และเป็นค่าเริ่มต้น
2. dict - เป็นการตัดคำโดยใช้พจานุกรมจาก thaiword.txt ใน corpus  (ความแม่นยำปานกลาง) จะคืนค่า False หากข้อความนั้นไม่สามารถตัดคำได้
3. longest-matching ใช้ Longest matching ในการตัดคำ
4. mm - ใช้ Maximum Matching algorithm ในการตัดคำภาษาไทย - API ชุดเก่า
5. newmm - ใช้ Maximum Matching algorithm ในการตัดคำภาษาไทย โค้ดชุดใหม่ โดยใช้โค้ดคุณ Korakot Chaovavanich  จาก https://www.facebook.com/groups/408004796247683/permalink/431283740586455/ มาพัฒนาต่อ
6. pylexto ใช้ LexTo ในการตัดคำ โดยเป็น Longest matching
7. deepcut ใช้ deepcut จาก https://github.com/rkcosmos/deepcut ในการตัดคำภาษาไทย
8. wordcutpy ใช้ wordcutpy (https://github.com/veer66/wordcutpy) ในการตัดคำ

คืนค่าเป็น ''list'' เช่น ['แมว','กิน']

**ตัวอย่าง**

```python
from pythainlp.tokenize import word_tokenize
text='ผมรักคุณนะครับโอเคบ่พวกเราเป็นคนไทยรักภาษาไทยภาษาบ้านเกิด'
a=word_tokenize(text,engine='icu') # ['ผม', 'รัก', 'คุณ', 'นะ', 'ครับ', 'โอ', 'เค', 'บ่', 'พวก', 'เรา', 'เป็น', 'คน', 'ไทย', 'รัก', 'ภาษา', 'ไทย', 'ภาษา', 'บ้าน', 'เกิด']
b=word_tokenize(text,engine='dict') # ['ผม', 'รัก', 'คุณ', 'นะ', 'ครับ', 'โอเค', 'บ่', 'พวกเรา', 'เป็น', 'คนไทย', 'รัก', 'ภาษาไทย', 'ภาษา', 'บ้านเกิด']
c=word_tokenize(text,engine='mm') # ['ผม', 'รัก', 'คุณ', 'นะ', 'ครับ', 'โอเค', 'บ่', 'พวกเรา', 'เป็น', 'คนไทย', 'รัก', 'ภาษาไทย', 'ภาษา', 'บ้านเกิด']
d=word_tokenize(text,engine='pylexto') # ['ผม', 'รัก', 'คุณ', 'นะ', 'ครับ', 'โอเค', 'บ่', 'พวกเรา', 'เป็น', 'คนไทย', 'รัก', 'ภาษาไทย', 'ภาษา', 'บ้านเกิด']
e=word_tokenize(text,engine='newmm') # ['ผม', 'รัก', 'คุณ', 'นะ', 'ครับ', 'โอเค', 'บ่', 'พวกเรา', 'เป็น', 'คนไทย', 'รัก', 'ภาษาไทย', 'ภาษา', 'บ้านเกิด']
```

#### sent_tokenize

ใช้ตัดประโยคภาษาไทย

```python
sent_tokenize(text,engine='whitespace+newline')
```

text คือ ข้อความในรูปแบบสตริง

engine คือ เครื่องมือสำหรับใช้ตัดประโยค

- whitespace ตัดประโยคจากช่องว่าง
- whitespace+newline ตัดประโยคจากช่องว่างและตัดจากการขึ้นบรรทัดใหม่

คืนค่า ออกมาเป็น list

#### WhitespaceTokenizer

ใช้ตัดคำ/ประโยคจากช่องว่างในสตริง

```python
>>> from pythainlp.tokenize import WhitespaceTokenizer
>>> WhitespaceTokenizer("ทดสอบ ตัดคำช่องว่าง")
['ทดสอบ', 'ตัดคำช่องว่าง']
```

#### isthai

ใช้เช็คข้อความว่าเป็นภาษาไทยทั้งหมดกี่ %

```python
isthai(text,check_all=False)
```

text คือ ข้อความหรือ list ตัวอักษร

check_all สำหรับส่งคืนค่า True หรือ False เช็คทุกตัวอักษร

**การส่งคืนค่า**

```python
{'thai':% อักษรภาษาไทย,'check_all':tuple โดยจะเป็น (ตัวอักษร,True หรือ False)}
```

#### Thai Character Clusters (TCC)

PyThaiNLP 1.4 รองรับ Thai Character Clusters (TCC) โดยจะแบ่งกลุ่มด้วย /

**เดติด**

TCC : Mr.Jakkrit TeCho

grammar : คุณ Wittawat Jitkrittum (https://github.com/wittawatj/jtcc/blob/master/TCC.g)

โค้ด : คุณ Korakot Chaovavanich 

**การใช้งาน**

```python
>>> from pythainlp.tokenize import tcc
>>> tcc.tcc('ประเทศไทย')
'ป/ระ/เท/ศ/ไท/ย'
```

#### Enhanced Thai Character Cluster (ETCC)

นอกจาก TCC แล้ว PyThaiNLP 1.4 ยังรองรับ Enhanced Thai Character Cluster (ETCC) โดยแบ่งกลุ่มด้วย /

**การใช้งาน**

```python
>>> from pythainlp.tokenize import etcc
>>> etcc.etcc('คืนความสุข')
'/คืน/ความสุข'
```

### keywords

ใช้หา keywords จากข้อความภาษาไทย

#### find_keyword

การทำงาน หาคำที่ถูกใช้งานมากกว่าค่าขั้นต่ำที่กำหนดได้ โดยจะลบ stopword ออกไป

```python
find_keyword(word_list,lentext=3)
```

word_list คือ list ของข้อความที่ผ่านการตัดคำแล้ว

lentext คือ จำนวนคำขั้นต่ำที่ต้องการหา keyword

คืนค่าออกมาเป็น dict

### tag

เป็น Part-of-speech tagging ภาษาไทย

```python
from pythainlp.tag import pos_tag
pos_tag(list,engine='old')
```

list คือ list ที่เก็บข้อความหลังผ่านการตัดคำแล้ว

engine คือ ชุดเครื่องมือในการ postaggers มี 2 ตัวดังนี้

1. old เป็น UnigramTagger (ค่าเริ่มต้น)
2. artagger เป็น RDR POS Tagger ละเอียดยิ่งกว่าเดิม รองรับเฉพาะ Python 3 เท่านั้น

### romanization

```python
from pythainlp.romanization import romanization
romanization(str,engine='pyicu')
```
มี 2 engine ดังนี้

- pyicu ส่งค่า Latin
- royin ใช้หลักเกณฑ์การถอดอักษรไทยเป็นอักษรโรมัน ฉบับราชบัณฑิตยสถาน (**หากมีข้อผิดพลาด ให้ใช้คำอ่าน เนื่องจากตัว royin ไม่มีตัวแปลงคำเป็นคำอ่าน**)

data :

รับค่า ''str'' ข้อความ 

คืนค่าเป็น ''str'' ข้อความ

**ตัวอย่าง**

```python
from pythainlp.romanization import romanization
romanization("แมว") # 'mæw'
```

### spell 

เป็น API สำหรับเช็คคำผิดในภาษาไทย 

```python
spell(word,engine='pn')
```

engine ที่รองรับ

- pn พัฒนามาจาก Peter Norvig (ค่าเริ่มต้น)
- hunspell ใช้ hunspell (ไม่รองรับ Python 2.7)

**ตัวอย่างการใช้งาน**

```python
from pythainlp.spell import *
a=spell("สี่เหลียม")
print(a) # ['สี่เหลี่ยม']
```
#### pn

```python
correction(word)
```

แสดงคำที่เป็นไปได้มากที่สุด

**ตัวอย่างการใช้งาน**

```python
from pythainlp.spell.pn import correction
a=correction("สี่เหลียม")
print(a) # ['สี่เหลี่ยม']
```

ผลลัพธ์

```
สี่เหลี่ยม
```

### pythainlp.number

```python
from pythainlp.number import *
```
จัดการกับตัวเลข โดยมีดังนี้

- thai_num_to_num(str)  - เป็นการแปลงเลขไทยสู่เลข
- thai_num_to_text(str) - เลขไทยสู่ข้อความ
- num_to_thai_num(str) - เลขสู่เลขไทย
- num_to_text(str) - เลขสู่ข้อความ
- text_to_num(str) - ข้อความสู่เลข
- numtowords(float) -  อ่านจำนวนตัวเลขภาษาไทย (บาท) รับค่าเป็น ''float'' คืนค่าเป็น  'str'

### collation

ใช้ในการเรียงลำดับข้อมูลภาษาไทยใน List

```python
from pythainlp.collation import collation
print(collation(['ไก่','ไข่','ก','ฮา'])) # ['ก', 'ไก่', 'ไข่', 'ฮา']
```

รับ list คืนค่า list

### date

#### now

รับเวลาปัจจุบันเป็นภาษาไทย

```python
from pythainlp.date import now
now() # '30 พฤษภาคม 2560 18:45:24'
```
### rank

#### rank

หาคำที่มีจำนวนการใช้งานมากที่สุด

```python
from pythainlp.rank import rank
rank(list)
```

คืนค่าออกมาเป็น dict

**ตัวอย่างการใช้งาน**

```python
>>> rank(['แมง','แมง','คน'])
Counter({'แมง': 2, 'คน': 1})
```

### change

#### แก้ไขปัญหาการพิมพ์ลืมเปลี่ยนภาษา

```python
from pythainlp.change import *
```

มีคำสั่งดังนี้

- texttothai(str) แปลงแป้นตัวอักษรภาษาอังกฤษเป็นภาษาไทย
- texttoeng(str) แปลงแป้นตัวอักษรภาษาไทยเป็นภาษาอังกฤษ

คืนค่าออกมาเป็น str

### soundex

เดติด คุณ Korakot Chaovavanich (จาก https://gist.github.com/korakot/0b772e09340cac2f493868da035597e8)

กฎที่รองรับในเวชั่น 1.4

- กฎการเข้ารหัสซาวน์เด็กซ์ของ  วิชิตหล่อจีระชุณห์กุล  และ  เจริญ  คุวินทร์พันธุ์ - LK82
- กฎการเข้ารหัสซาวน์เด็กซ์ของ วรรณี อุดมพาณิชย์ - Udom83

**การใช้งาน**

```python
>>> from pythainlp.soundex import LK82
>>> print(LK82('รถ'))
ร3000
>>> print(LK82('รด'))
ร3000
>>> print(LK82('จัน'))
จ4000
>>> print(LK82('จันทร์'))
จ4000
>>> print(Udom83('รถ'))
ร800000
```

### Meta Sound ภาษาไทย

```
Snae & Brückner. (2009). Novel Phonetic Name Matching Algorithm with a Statistical Ontology for Analysing Names Given in Accordance with Thai Astrology. Retrieved from https://pdfs.semanticscholar.org/3983/963e87ddc6dfdbb291099aa3927a0e3e4ea6.pdf
```

**การใช้งาน**

```python
>>> from pythainlp.MetaSound import *
>>> MetaSound('คน')
'15'
```

### sentiment

เป็น Sentiment analysis ภาษาไทย ใช้ข้อมูลจาก [https://github.com/wannaphongcom/lexicon-thai/tree/master/ข้อความ/](https://github.com/wannaphongcom/lexicon-thai/tree/master/ข้อความ/)

```python
from pythainlp.sentiment import sentiment
sentiment(str)
```

รับค่า str ส่งออกเป็น pos , neg หรือ neutral

### Util

การใช้งาน

```python
from pythainlp.util import *
```

#### ngrams

สำหรับสร้าง n-grams 

```python
ngrams(token,num)
```

- token คือ list
- num คือ จำนวน ngrams

#### bigrams

สำหรับสร้าง bigrams

```python
bigrams(token)
```

- token คือ list

#### trigram

สำหรับสร้าง trigram

```python
trigram(token)
```

- token คือ list

#### normalize

ซ่อมข้อความภาษาไทย เช่น กี่่่ ไปเป็น กี่

```python
normalize(text)
```

**ตัวอย่าง**

```python
>>> print(normalize("เเปลก")=="แปลก") # เ เ ป ล ก กับ แปลก
True
```

### Corpus

#### WordNet ภาษาไทย

เรียกใช้งาน

```python
from pythainlp.corpus import wordnet
```

**การใช้งาน**

API เหมือนกับ NLTK โดยรองรับ API ดังนี้

- wordnet.synsets(word)
- wordnet.synset(name_synsets)
- wordnet.all_lemma_names(pos=None, lang="tha")
- wordnet.all_synsets(pos=None)
- wordnet.langs()
- wordnet.lemmas(word,pos=None,lang="tha")
- wordnet.lemma(name_synsets)
- wordnet.lemma_from_key(key)
- wordnet.path_similarity(synsets1,synsets2)
- wordnet.lch_similarity(synsets1,synsets2)
- wordnet.wup_similarity(synsets1,synsets2)
- wordnet.morphy(form, pos=None)
- wordnet.custom_lemmas(tab_file, lang)

**ตัวอย่าง**

```python
>>> from pythainlp.corpus import wordnet
>>> print(wordnet.synsets('หนึ่ง'))
[Synset('one.s.05'), Synset('one.s.04'), Synset('one.s.01'), Synset('one.n.01')]
>>> print(wordnet.synsets('หนึ่ง')[0].lemma_names('tha'))
[]
>>> print(wordnet.synset('one.s.05'))
Synset('one.s.05')
>>> print(wordnet.synset('spy.n.01').lemmas())
[Lemma('spy.n.01.spy'), Lemma('spy.n.01.undercover_agent')]
>>> print(wordnet.synset('spy.n.01').lemma_names('tha'))
['สปาย', 'สายลับ']
```

#### stopword ภาษาไทย

```python
from pythainlp.corpus import stopwords
stopwords = stopwords.words('thai')
```

#### ชื่อประเทศ ภาษาไทย

```python
from pythainlp.corpus import country
country.get_data()
```

#### ตัววรรณยุกต์ในภาษาไทย

```python
from pythainlp.corpus import tone
tone.get_data()
```

#### ตัวพยัญชนะในภาษาไทย

```python
from pythainlp.corpus import alphabet
alphabet.get_data()
```

#### รายการคำในภาษาไทย

```python
from pythainlp.corpus.thaiword import get_data # ข้อมูลเก่า
get_data()
from pythainlp.corpus.newthaiword import get_data # ข้อมูลใหม่
get_data()
```

#### provinces

สำหรับจัดการชื่อจังหวัดในประเทศไทย

##### get_data

รับข้อมูลชื่อจังหวัดในประเทศไทบ

```python
get_data()
```

คือค่าออกมาเป็น list

##### parsed_docs

สำหรับใช้ Tag ชื่อจังหวัดในประเทศไทย

```python
parsed_docs(text_list)
```

text_list คือ ข้อความภาษาไทยที่อยู่ใน list โดยผ่านการตัดคำมาแล้ว

**ตัวอย่าง**

```python
>>> d=['หนองคาย', 'เป็น', 'เมือง', 'น่าอยู่', 'อันดับ', 'ต้น', 'ๆ', 'ของ', 'โลก', 'นอกจากนี้', 'ยัง', 'มี', 'เชียงใหม่']
>>> parsed_docs(d)
["[LOC : 'หนองคาย']", 'เป็น', 'เมือง', 'น่าอยู่', 'อันดับ', 'ต้น', 'ๆ', 'ของ', 'โลก', 'นอกจากนี้', 'ยัง', 'มี', "[LOC : 'เชียงใหม่']"]
```

#### ConceptNet

เครื่องมือสำหรับ ConceptNet.

**ค้นหา edges**

```python
edges(word,lang='th')
```

return dict

#### TNC

สำหรับใช้จัดการกับ Thai National Corpus (http://www.arts.chula.ac.th/~ling/TNC/index.php)

##### word_frequency

ใช้วัดความถี่ของคำ

```python
word_frequency(word,domain='all')
```

word คือ คำ

domain คือ หมวดหมู่ของคำ

มีหมวดหมู่ดังนี้

- all
- imaginative
- natural-pure-science
- applied-science
- social-science
- world-affairs-history
- commerce-finance
- arts
- belief-thought
- leisure
- others

เขียนโดย นาย วรรณพงษ์  ภัททิยไพบูลย์
