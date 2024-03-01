---
source-git-commit: d105a5b7d81aa14144b9d01f28a5e24c1110ae6c
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 7%

---
# 建立型別規則以支援按一下清單取消訂閱：

**1. 建立新的型別規則：**

* 在導覽樹狀結構中，按一下「新增」以建立新的型別

![影像](/help/assets/CreatingTypologyRules1.png)

**2. 繼續設定型別規則：**

* 規則型別：控制
* 階段：在鎖定開始時
* 頻道：電子郵件
* 等級：您的選擇
* 作用中


![影像](/help/assets/CreatingTypologyRules2.png)


**將型別規則的javascript程式碼：**


>[!NOTE]
>
>下述程式碼僅供範例參照。
>此範例詳細說明如何：
>* 設定URL List-Unsubscribe並將新增標題或附加現有的mailto：引數並將其取代為： &lt;mailto..>>， https://...
>* 在List-Unsubscribe-Post標頭中新增
>貼文URL範例使用var headerUnsubUrl = &quot;https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=&lt;%= recipient.cryptedId %>&quot;÷
>* 您可以新增其他引數（例如&amp;service = ...）
>


```
// Function to add or replace a header in the provided headers 
function addHeader(headers, header, value)  { 
    
  // Create the new header line 
  var headerLine = header + ": " + value; 
    
  // Create a regular expression to find the specified header 
  var regExp = new RegExp(header + ":(.*)$", "i") 
    
  // Split the headers into individual lines 
  var headerLines = headers.split("\n"); 
    
  // Loop through each line 
  for (var i=0; i < headerLines.length; i++) { 
      
    // Check if the specified header exists 
    var match = headerLines[i].match(regExp) 
      
    // If it exists 
    if ( match != null ) { 
        
      // Replace the existing header line 
      headerLines[i] = headerLine; 
        
      // Return the modified headers 
      return headerLines.join("\n"); 
    } 
  } 
    
  // If the header does not exist, add the new header line 
  headerLines.push(headerLine); 
    
  // Return the modified headers 
  return headerLines.join("\n"); 
} 
  
// Function to get the value of a specified header from the provided headers 
function getHeader(headers, header) { 
    
  // Create a regular expression to find the specified header 
  var regExp = new RegExp(header + ":(.*)$", "i") 
    
  // Split the headers into individual lines 
  var headerLines = headers.split("\n"); 
    
  // Loop each line 
  for each (line in headerLines) { 
      
    // Check if the specified header exists 
    var match = line.match(regExp); 
      
    // If it exists 
    if ( match != null ) { 
        
      // Return the header value, removing leading whitespace 
      return match[1].replace(/^\s*/, ""); 
    } 
  } 
    
  // If the header does not exist, return an empty string 
  return ""; 
} 
  
  
// Define the unsubscribe URL 
var headerUnsubUrl = "https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=<%= recipient.cryptedId %>"; 
  
// Get the value of the List-Unsubscribe header 
var headerUnsub = getHeader(delivery.mailParameters.headers, "List-Unsubscribe"); 
  
// If the List-Unsubscribe header does not exist 
if ( headerUnsub === "" ) { 
  // Add the List-Unsubscribe header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe", "<"+headerUnsubUrl+">"); 
} 
// If the List-Unsubscribe header exists and contains 'mailto' 
else if(headerUnsub.search('mailto')){ 
  // Replace the existing List-Unsubscribe header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe", "<"+headerUnsubUrl+">"); 
} 
  
// Get the value of the List-Unsubscribe-Post header 
var headerUnsubPost = getHeader(delivery.mailParameters.headers, "List-Unsubscribe-Post"); 
  
// If the List-Unsubscribe-Post header does not exist 
if ( headerUnsubPost === "" ) { 
  // Add the List-Unsubscribe-Post header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe-Post", "List-Unsubscribe=One-Click"); 
} 
  
// Return true to indicate success 
return true; 
```


![影像](/help/assets/CreatingTypologyRules3.png)

**3.將新規則新增至電子郵件的「型別」 （預設型別為確定）：**

![影像](/help/assets/CreatingTypologyRules4.png)

**4.準備新傳遞（確認傳遞屬性中的其他SMTP標頭為空白）**

![影像](/help/assets/CreatingTypologyRules5.png)

**5.在傳遞準備期間檢查您的新型別規則是否已套用。**

![影像](/help/assets/CreatingTypologyRules6.png)



**6. 驗證List-Unsubscribe是否存在。**

![影像](/help/assets/CreatingTypologyRules7.png)
