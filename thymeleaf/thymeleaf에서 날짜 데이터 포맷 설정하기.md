### thymeleaf에서 날짜 데이터 포맷 설정하기

#### Date 포맷일 때 -> #dates.format(날짜, 형식)
```HTML
 <td th:text="${#dates.format('20230331 00:00:00','yyyy-MM-dd')}"></td>
```

#### String 포맷일 때 -> #temporals.format(날짜, 형식)
```HTML
 <td th:text="${#temporals.format('20230331','yyyy-MM-dd')}"></td>
```

#### dates의 여러 기능
```
// 값을 standard 포맷에 맞게 변환
${#dates.format(date)}
${#dates.arrayFormat(datesArray)}
${#dates.listFormat(datesList)}
${#dates.setFormat(datesSet)}

// 값을 ISO8601 포맷에 맞게 변환
${#dates.formatISO(date)}
${#dates.arrayFormatISO(datesArray)}
${#dates.listFormatISO(datesList)}
${#dates.setFormatISO(datesSet)}

// 값을 지정된 포맷에 맞게 변환
${#dates.format(date, 'dd/MMM/yyyy HH:mm')}
${#dates.arrayFormat(datesArray, 'dd/MMM/yyyy HH:mm')}
${#dates.listFormat(datesList, 'dd/MMM/yyyy HH:mm')}
${#dates.setFormat(datesSet, 'dd/MMM/yyyy HH:mm')}

// dates 객체의 특정 요소값
${#dates.day(date)}                    
${#dates.month(date)}                  
${#dates.monthName(date)}              
${#dates.monthNameShort(date)}         
${#dates.year(date)}                   
${#dates.dayOfWeek(date)}              
${#dates.dayOfWeekName(date)}          
${#dates.dayOfWeekNameShort(date)}     
${#dates.hour(date)}                   
${#dates.minute(date)}                 
${#dates.second(date)}                 
${#dates.millisecond(date)}            

// dates 객체 생성
${#dates.create(year,month,day)}
${#dates.create(year,month,day,hour,minute)}
${#dates.create(year,month,day,hour,minute,second)}
${#dates.create(year,month,day,hour,minute,second,millisecond)}
${#dates.createNow()}
${#dates.createNowForTimeZone()}
${#dates.createToday()}
${#dates.createTodayForTimeZone()}
```



참고 (thymeleaf 공식 깃허브)
https://github.com/thymeleaf/thymeleaf-extras-java8time
