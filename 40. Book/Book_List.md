---
cssclasses:
  - cards
  - cards-1-1
  - cards-cover
tags:
  - List
---

```dataview 
table without id
	book_title as "제목",
	author[0] as "작가", 
	("![|100](" + cover_url + ")") as "표지", 
	tags[1] as "태그",
	finish_date as "읽은 날짜", 
	book_note as "독서 노트"

from "40. Book/41. Book_list"



sort finish_date

```


