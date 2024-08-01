---
date_daily: <% tp.file.title.slice(0,10) %>
achievement: 
reading_book: 
emotion: 
important_date: false
tags:
  - daily
daily_review:
---
<%* 
	const currentMoment = moment(tp.file.title, "YYYY-MM-DD");
	tR += '❮ '; tR += '[[' + currentMoment.format('YYYY|YYYY년') + ']]' + ' / '; 
	tR += '[[' + currentMoment.format('YYYY-MM|MM월') + ']]' + ' / '; 
	tR += '[[' + currentMoment.format('gggg-[W]ww') + '|' + currentMoment.format('ww[주]') + ']]'; 
	tR += ' ❯'; 
	tR += '\n';
	tR += '❮❮ '; 
	currentMoment.add(-1,'days'); 
	tR += '[[' + currentMoment.format('YYYY-MM-DD(ddd)') + ']]' + ' | '; currentMoment.add(1,'days'); 
	tR += currentMoment.format('YYYY-MM-DD(ddd)') + ' | '; currentMoment.add(1,'days'); 
	tR += '[[' + currentMoment.format('YYYY-MM-DD(ddd)') + ']]'; currentMoment.add(-1,'days'); 
	tR += ' ❯❯'; %>


<% tp.web.daily_quote() %>

## 내일 기억할 일 
-

## 오늘 기억할 일
<%* 
	let yesterday = "10. Planner/11. Daily/" + tp.date.now("YYYY-MM-DD(ddd)", -1, tp.file.title, "YYYY-MM-DD(ddd)"); 
	let section = "## 내일 기억할 일"; 
	let should_include = false; 
	let sectionContent = ""; 
	let yfile = tp.file.find_tfile(yesterday); 
	if(yfile) { const content = await app.vault.read(yfile); if(content.includes(section)) { let startIndex = content.indexOf(section) + section.length; 
	let endIndex = content.indexOf('\n##', startIndex); endIndex = endIndex === -1 ? content.length : endIndex; sectionContent = content.substring(startIndex, endIndex).trim(); should_include = sectionContent.length > 0; } } tR += should_include ? sectionContent : "없음"; 
%>
## 아침 
### 오늘의 확언 
- 
### 오늘의 목표 
- [ ] 
- [ ] 

### 할 일 추가하기 
- [ ] 

## 오늘 끝내야 할 일 
```tasks
due on or before <% tp.file.title.slice(0,10) %> 
filter by function task.file.folder.includes("Planner") 
filter by function !task.file.folder.includes("templates") 
not done 
sort by priority 
```
### 반복 할 일 
```tasks
is recurring
due on or before <% tp.file.title.slice(0,10) %> 
not done
```

### 언젠가 할 일 
```tasks 
no due date 
not done 
description regex does not match /^$/
is not recurring
``` 
### 오늘 완료한 일 
```tasks
done <% tp.file.title.slice(0,10) %> 
``` 
## 독서 
- 읽은 책 
- 읽은 페이지 
## 운동 
- 
## 하루 마무리 
### 오늘 배운 것들 
- 
### 오늘 감사한 일 
>[!note] 


### 일기 
## 오늘 작성한 노트 
```dataview 
List FROM "" WHERE file.cday = date("<% tp.date.now('YYYY-MM-DD') %>") SORT file.ctime desc 
``` 
## 오늘 수정한 노트 
 ```dataview 
 List FROM "" WHERE file.mday = date("<% tp.date.now('YYYY-MM-DD') %>") SORT file.mtime desc 
 ```
 ## 오늘의 요약
>[!summary]
>
>^summary

