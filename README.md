# BCTP  
### Bullying Consequence Transparency Platform

A web-based educational platform designed to help users understand the legal consequences of bullying through case references, legal education, quizzes, and reflection submission.

---

## Project Website

<a href="https://voidstu.github.io/BCTP/">
  <img width="200" height="200" src="https://github.com/user-attachments/assets/bee2e114-6129-4408-9ebc-56416ff4ff8b" />
</a>

**Live Demo:**  
https://voidstu.github.io/BCTP/

---

# Project Structure

## Front-end (GitHub Pages)

The front-end is built using static HTML, CSS, and JSON-based data files.

---

## Data Files

### [LawJson.json](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/LawJson.json)
Contains legal regulations and consequences related to different types of bullying behavior.

### [CaseJson.json](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/CaseJson.json)
Contains real-world bullying-related cases for educational reference.

### [QuizJson.json](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/QuizJson.json)
Stores quiz questions used to evaluate user understanding of laws and cases.

---

## Pages

### [index.html](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/index.html)
Platform homepage introducing the project purpose and system workflow.

---

### [query.html](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/query.html)
Allows users to select bullying behaviors and view:

- Relevant laws
- Legal consequences
- Related real-life cases

---

### [quiz.html](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/quiz.html)
Knowledge assessment page.

- Quiz questions are dynamically generated from selected behavior data
- Tests user understanding of legal consequences
- Users must score **60 points or above** to proceed

---

### [form.html](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/form.html)
Reflection submission page where users provide:

- School name
- Student name
- Reflection content
- Selected bullying behavior

The submission is then stored in the record system.

---

## Styling

### [style.css](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/style.css)

Controls:

- Layout structure
- Typography
- Responsive design
- Visual theme consistency

---

## Platform Logo

<img width="200" height="200" src="https://github.com/user-attachments/assets/c826f20c-f673-4e16-ab3b-cf505c000760">

---

# Back-end (Google Apps Script)

The platform uses **Google Apps Script + Google Sheets** as its backend storage system.

It receives data submitted from `form.html` and stores:

- Timestamp
- School Name
- Student Name
- Reflection Content
- Behavior Type

---

## Apps Script Code

```javascript
const SHEET_NAME = "record";  // 設定要操作的試算表工作表名稱

function doPost(e) {
  const responseJson = (data) => 
    ContentService.createTextOutput(JSON.stringify(data))
                  .setMimeType(ContentService.MimeType.JSON);

  // 檢查是否收到有效的 POST 請求數據，若沒有則返回錯誤
  if (!e || !e.postData || !e.postData.contents) {
    return responseJson({result: 'error', message: '未收到任何數據'});
  }
  
  let data;
  try {
    // 解析收到的 JSON 數據
    data = JSON.parse(e.postData.contents);
  } catch (error) {
    return responseJson({result: 'error', message: '無效的 JSON 格式'});
  }

  try {
    const ss = SpreadsheetApp.getActiveSpreadsheet();
    const sheet = ss.getSheetByName(SHEET_NAME);
    
    // 檢查工作表是否存在
    if (!sheet) {
        return responseJson({result: 'error', message: `找不到名為 "${SHEET_NAME}" 的工作表。`});
    }
    
    // 準備數據並將其寫入工作表
    const rowData = [
      new Date(),
      data.school,
      data.studentName,
      data.reflection,
      data.behaviors
    ];

    sheet.appendRow(rowData);
    return responseJson({result: 'success', status: '數據已成功記錄'});

  } catch (error) {
    return responseJson({result: 'error', message: `伺服器錯誤: ${error.toString()}`});
  }
}
```

---

# Workflow Overview

1. User enters the platform  
2. Selects bullying behavior  
3. Reviews related laws and real cases  
4. Completes knowledge quiz  
5. Achieves passing score (60+)  
6. Writes reflection report  
7. Reflection is stored in Google Sheets

---

# Purpose

BCTP promotes awareness of bullying consequences by combining:

- Legal education
- Real case analysis
- Knowledge assessment
- Reflective learning
- Digital accountability records

The goal is to encourage behavioral correction through understanding rather than punishment alone.
