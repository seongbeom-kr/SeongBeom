---
weekly_summary: 
tags:
  - weekly
---
## 지난주 피드백
-  
-  

## 이번주 계획
<%* 
function getPreviousWeek(year, week) { 
	if (week === 1) { 
		year -= 1; 
		let lastWeekOfYear = getISOWeekNumber(new Date(year, 11, 28)); 
		return `${year}-W${String(lastWeekOfYear).padStart(2, '0')}`; 
	} else { 
		week -= 1; 
		return `${year}-W${String(week).padStart(2, '0')}`; } } 

function getISOWeekNumber(date) { 
	let day = new Date(date.getFullYear(), date.getMonth(), date.getDate()); 
	let dayNum = day.getDay() || 7; 
	day.setDate(day.getDate() + 4 - dayNum); 
	let yearStart = new Date(day.getFullYear(), 0, 1); 
	return Math.ceil((((day - yearStart) / 86400000) + 1) / 7); 
	} 
let [year, week] = tp.file.title.split("-W").map(Number); let lastWeek = getPreviousWeek(year, week); 
let lastWeekPath = "10. Planner/12. Weekly/" + lastWeek; let section = "## 다음 주 계획"; 
let should_include = false; 
let sectionContent = ""; 
let lwfile = tp.file.find_tfile(lastWeekPath); 

if(lwfile) { const content = await app.vault.read(lwfile); if(content.includes(section)) { 
let startIndex = content.indexOf(section) + section.length;
let endIndex = content.indexOf('\n##', startIndex); endIndex = endIndex === -1 ? content.length : endIndex; sectionContent = content.substring(startIndex, endIndex).trim(); 
should_include = sectionContent.length > 0; } } 
tR += should_include ? sectionContent : "없습니다😀";

%>

---
## 이번주 요약
| 요일  | 날짜                                                                                     | 내용                          |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
| --- | -------------------------------------------------------------------------------------- | --------------------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 월   | `$=moment("2024-W", "YYYY-[W]WW").startOf('isoWeek').format("MM-DD")`                | ![[2024-07-15(월)#^summary]] |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
| 화   | `$=moment("2024-W", "YYYY-[W]WW").startOf('isoWeek').add(1, 'days').format("MM-DD")` | ![[2024-07-16(화)#^summary]] |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
| 수   | `$=moment("2024-W", "YYYY-[W]WW").startOf('isoWeek').add(2, 'days').format("MM-DD")` | ![[2024-07-17(수)#^summary]] |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
| 목   | `$=moment("2024-W", "YYYY-[W]WW").startOf('isoWeek').add(3, 'days').format("MM-DD")` | ![[2024-07-18(목)#^summary]] |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
| 금   | `$=moment("2024-W", "YYYY-[W]WW").startOf('isoWeek').add(4, 'days').format("MM-DD")` | ![[2024-07-19(금)#^summary]] |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
| 토   | `$=moment("2024-W", "YYYY-[W]WW").startOf('isoWeek').add(5, 'days').format("MM-DD")` | ![[2024-07-20(토)#^summary]] |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
| 일   | `$=moment("2024-W", "YYYY-[W]WW").startOf('isoWeek').add(6, 'days').format("MM-DD")` | ![[2024-07-21(일)#^summary]] |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |


---
## 데일리 리뷰
```dataviewjs 
const currentNoteTitle = dv.current().file.name;
const weekNumberMatch = currentNoteTitle.match(/(\d{4}-W\d{2})/); 

if (weekNumberMatch) {
	const weekNumber = weekNumberMatch[0]; 
	const dailyNoteFolder = '"10. Planner/11. Daily"'; 
	
	dv.pages(dailyNoteFolder) 
		.where(page => { 
			const pageDate = moment(page.file.name, "YYYY-MM-DD(ddd)"); 
			return pageDate.isValid() && pageDate.isoWeek() === moment(weekNumber, "YYYY-[W]WW").isoWeek(); 
			}) 
			.forEach(page => { 
			const dailyReview = page.daily_review || "없음"; dv.paragraph(`**${page.file.name}**: ${dailyReview}`); }); 
} else { 
			dv.paragraph("이 노트의 제목에 ISO 주 정보가 포함되지 않았습니다."); 
}
```
