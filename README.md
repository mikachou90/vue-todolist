# vue-todo-list

使用 vue.js 3 搭配 vue-router 和 vite 寫成 todo-list 專案

Live Demo <https://mikachou90.github.io/vue-todolist/>

## 專案介紹

![signup picture](/public/signup%20page.png)

【註冊功能 】

任何使用者皆可建立一個帳號，密碼長度須至少6個字數。註冊成功會跳出 alart 通知，並導回登入頁面，需再次輸入帳號及密碼即可登入。

若重複註冊同一個 email 則會跳出 alart 通知，並導回登入頁面。

---

![login picture](/public/login%20page.png)

【登入功能】

輸入已註冊帳號即可登入。登入成功會跳出 alart 通知後導向主頁面。

---

![todo picture](/public/todo%20page.png)

【新增/確認/刪除 待辦事項】

1. 上方 input 欄可輸入待辦事項，按黑色+按鈕即可新增待辦事項。
2. 下方欄位會顯示待辦事項，欄位上方有三個按鈕「全部」、「待完成」、「已完成」按鈕可以切換瀏覽不同狀態的待辦事項。
3. 每個待辦事項前方有正方形按鈕，可以切換事項是否完成的狀態。正方形為待完成、橘色 check icon 為已完成。已完成的事項會以淺灰色和刪除號呈現，已完成的事項再次點選可以切換成未完成狀態。
4. 每個待辦事項後方的 x 按鈕為刪除待辦事項。
5. 最下方會顯示尚有多少個待辦事項。
6. 右上方會顯示使用者名稱。
7. 點選右上角「登出」即可登出頁面，並導回登入頁面。
