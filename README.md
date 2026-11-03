# 專題報告


## 1. 名稱
- 科系 : 資訊工程系
- 姓名 : 薛智鴻
- 專題名稱：日常代辦清單
- 學號：22113115
- GitHub連結：https://github.com/User-166266366/-1/blob/main/index.html

---

## 2. 目的

防止人腦記憶不足的問題而設計，採用簡單明瞭的按鍵及輸入欄為使用者提供直接且方便的備忘紀錄。

---

## 3. 功能

輸入欄:可在其中輸入文字，點選新增後可繼續輸入下一備忘錄內容。
按鈕:有"新增"及"刪除"兩按鈕，新增按鈕能新增備忘項目並將輸入欄中的文字套入其中，而刪除則是用於刪除新增的備忘項目。
勾選框(checkbox):勾選後備忘項目的文字顏色會轉淡，取消勾選則使文字恢復正常。

- 
- 
- 

---

## 4. 程式運作流程說明

> 提示：依程式執行順序，分步驟說明。每個步驟都要有「說明、程式碼、程式碼說明」，步驟標題請標出對應的函式名稱。
> 步驟不夠請自行複製下面的格式增加。

### 步驟 1：（做什麼事）（`函式名稱`）

說明：
首先，我們需要輸入文字於輸入框中，並於點擊按鈕後將新增的代辦項目加入清單並清除輸入欄內的內容。
為了避免輸入後還需要重新點擊輸入欄才能打字，因此在新增項目後，會自動焦聚在輸入欄上，讓使用者能不間斷的輸入代辦內容並新增項目。
同時為了誤觸空白鍵或者空內容，因此會自動清除輸入欄內，前後的文字空白，而在接收到空白內容或者無輸入時也會擋下來。
在無輸入內容時，則系統會跳出警告。

```js
form.addEventListener("submit", event => {
      event.preventDefault();
      const text = input.value.trim(); //清除前後文字空白 (用於代辦清單內容的文字輸入)

    //防止使用者輸入空白或沒有輸入內容，若沒有輸入內容則會自動將焦點回到輸入框
      if (!text) {
        input.focus();
        return;
      }

      //將新增的待辦事項加入清單，並將完成狀態設為 false(見第435行程式碼)
      tasks.push({
        id: createId(),
        text,
        completed: false
      });

      saveTasks();
      renderTasks();
      form.reset(); //清除輸入框裡的字
      input.focus(); //自動聚焦在輸入框上，讓使用者能繼續輸入下一件待辦事項
    });
```

程式碼說明：

### 步驟 2：（做什麼事）（`函式名稱`）

說明：
接下來，在代辦清單中，系統會記錄當前未完成的項目數量並顯示。
當清單中的項目被點擊時(被打勾)，則會自動減少一個未完成事項。
當清單中未擁有任何項目時，會跳出"目前沒有待辦，先新增一件小事吧。"文字。
為了能及時同步資訊，每次點擊checkbox時(不論打勾或是取消打勾)，網頁都會即時更新，確保頁面與狀態的同步。
當點擊checkbox時，還會閱讀已完成的事項內容，以避免視障使用者的不方便。

```js
//計算目前「未完成」的數量
      const remaining = tasks.filter(task => !task.completed).length;
      taskCount.textContent = `尚有 ${remaining} 件未完成`; //更新未完成數量
      emptyState.hidden = tasks.length > 0; //偵測是否有待辦事項，若沒有則顯示「目前沒有待辦，先新增一件小事吧。」(見程式碼第 343 行的emptyState 所指引的元素(位於第328行的empty-state))

      for (const task of tasks) {
        const item = document.createElement("li");
        item.className = `task-item${task.completed ? " is-complete" : ""}`;//設定CSS類別名稱

        //偵測checkbox變化
        const checkbox = document.createElement("input");
        checkbox.className = "task-check";
        checkbox.type = "checkbox";
        checkbox.checked = task.completed;
        checkbox.setAttribute("aria-label", `完成：${task.text}`); //無障礙設計，讓螢幕閱讀器讀出「完成：待辦事項內容」
        checkbox.addEventListener("change", () => {
          task.completed = checkbox.checked; //狀態更改 (勾選及取消勾選)
          saveTasks(); //保存狀態
          renderTasks(); //重新整理頁面
        });
```

程式碼說明：

### 步驟 3：（做什麼事）（`函式名稱`）

說明：點擊後會刪除對應的代辦項目，同時讓網頁閱讀使用者所刪除的代辦內容。

```js
// 刪除按鈕
        const deleteButton = document.createElement("button");
        deleteButton.className = "delete-button";
        deleteButton.type = "button";
        deleteButton.textContent = "刪除";
        deleteButton.setAttribute("aria-label", `刪除：${task.text}`); //無障礙設計，讓螢幕閱讀器讀出「刪除：待辦事項內容」
        deleteButton.addEventListener("click", () => {
          tasks = tasks.filter(item => item.id !== task.id);
          saveTasks();
          renderTasks();
        });
```

程式碼說明：

---
