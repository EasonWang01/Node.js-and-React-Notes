# Cookie 與 LocalStorage

## Localstorage 會被子網域存取或寫入嗎

`localStorage` 是同源策略 (Same-origin policy) 的範疇。這意味著網頁只能存取或修改與該頁面完全相同原始位置（協議 + 域名 + 端口）的資料。

具體來說：

* `a.com` 與 `b.a.com` 不屬於同一個源，因此他們不能共享 `localStorage`。
* `a.com` 與 `a.com`（不同的路徑或協議）屬於同一個源，所以他們可以共享 `localStorage`。

這意味著子域（例如 `sub.a.com`）不能存取主域（例如 `a.com`）的 `localStorage` 資料，反之亦然。同樣地，不同的子域之間（例如 `sub1.a.com` 與 `sub2.a.com`）也無法存取彼此的 `localStorage`。

這種行為是為了網頁的安全。如果不使用同源策略，那麼惡意的網站或子域可能會試圖存取其他網站的資料，這會帶來顯著的安全風險。

## Cookie 會被子網域存取或寫入嗎

是的，與 `localStorage` 不同，cookies 可以跨主域名和子域名共享，但這取決於如何設置 cookie 的 "Domain" 屬性。

當你設置一個 cookie 時，你可以指定一個 "Domain" 屬性。以下是一些情境：

1. **沒有指定 Domain 屬性**: 如果你沒有明確指定 "Domain" 屬性，則 cookie 默認僅對設置它的完全資格域名（FQDN）可用。例如，如果 `sub.a.com` 設置了一個 cookie，但沒有指定 "Domain"，那麼只有 `sub.a.com` 可以讀取該 cookie，而 `a.com` 或其他子域（例如 `other.sub.a.com`）則無法讀取它。
2. **指定 Domain 屬性為 `.a.com`**: 如果一個 cookie 被設置為 "Domain=.a.com"（注意領先的點），那麼該 cookie 對 `a.com` 及其所有子域都是可見的。這意味著 `a.com`、`sub.a.com` 和 `other.a.com` 都可以讀取（和修改）這個 cookie。

但要注意，子域可以為父域設置 cookie，但父域不能為子域設置 cookie。也就是說，`a.com` 不能直接為 `sub.a.com` 設置一個 cookie，除非該 cookie 的域也被設置為 `.a.com`。

由於子域能夠讀取和寫入主域的 cookies（如果 "Domain" 屬性允許），這可能導致安全問題，因此需要謹慎管理和設置 cookies。這也是為什麼很多網站選擇將其安全敏感的操作（例如購物車、帳戶管理等）和其他非敏感內容分開到不同的域或子域上，以確保敏感資料的安全性。

#### Cookie 的主要屬性：

1. **Name**：Cookie 的名稱。
2. **Value**：Cookie 的值。
3. **Domain**：Cookie 適用的域。如果沒有設置，默認為設置此 cookie 的域。
4. **Path**：Cookie 適用的路徑。只有匹配此路徑的頁面才能讀取該 cookie。默認為 `/`，意味著整個域下的所有頁面都可以讀取該 cookie。
5. **Expires/Max-Age**：Cookie 的到期日期或最大壽命。如果沒有設置，則該 cookie 是一個 session cookie，並且會在瀏覽器關閉時消失。
6. **Secure**：如果設置了此屬性，則 cookie 只會在 HTTPS 連接中被傳送。
7. **HttpOnly**：如果設置了此屬性，則 cookie 不可以被 JavaScript 腳本讀取。這增加了防止跨站腳本攻擊 (XSS) 的安全性。
8. **SameSite**：這可以讓 cookie 在跨站請求中避免被發送，從而增強了對某些類型跨站請求偽造 (CSRF) 攻擊的防禦。它可以有以下值：
   * **Strict**：Cookie 只會在同一站點請求中被發送。
   * **Lax**：Cookie 在跨站子請求中（例如圖片或連結）不會被發送，但會在頂級導航中被發送。
   * **None**：Cookie 在跨站請求中會被發送。通常需要 `Secure` 屬性伴隨。

\
