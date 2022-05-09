# 建立太空遊戲 Part 4：加入雷射與碰撞偵測

## 課前測驗

[課前測驗](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/35?loc=zh_tw)

這堂課中，你會學會如何在 JavaScript 上發射雷射光！我們需要在遊戲中新增兩項東西：

- **雷射光**：這束雷射會從英雄艦艇垂直往上移動
- **碰撞偵測**，除了完成*射擊*這項能力，我們還會新增幾項遊戲規則：
   - **雷射擊中敵人**：被雷射擊中的敵人會死亡
   - **雷射擊中畫面最上方**：雷射擊中到畫面最上方會消失
   - **敵人碰觸到英雄**：敵人與英雄在互相碰觸時皆會被摧毀
   - **敵人碰觸到畫面最下方**：敵人碰觸到畫面最下方時，該敵人與英雄會被摧毀

簡單來說，你這位*英雄*需要在敵人到達畫面最下方之前，使用雷射擊毀它們。

✅ 做點關於第一款電腦遊戲的研究。它有哪些功能？

讓我們成為英雄吧！

## 碰撞偵測

我們該如何進行碰撞偵測呢？我們需要將遊戲物件視為移動中的矩形。你或許會問為什麼？這是因為，繪製遊戲物件的圖片皆為矩形：它有一組 `x` 與 `y`、`width` 與 `height`。

若兩個矩形，舉例來說：英雄與敵人*相交*了，這就是一次碰撞。至於會發生什麼事取決於遊戲規則。要製作碰撞偵測，你需要這些步驟：

1. 取得表達遊戲物件為矩形的方法，好比是：

   ```javascript
   rectFromGameObject() {
     return {
       top: this.y,
       left: this.x,
       bottom: this.y + this.height,
       right: this.x + this.width
     }
   }
   ```

2. 一個比較函式，這個函式可以如下：

   ```javascript
   function intersectRect(r1, r2) {
     return !(r2.left > r1.right ||
       r2.right < r1.left ||
       r2.top > r1.bottom ||
       r2.bottom < r1.top);
   }
   ```

## 我們該如何摧毀物件

要摧毀遊戲物件，你需要讓遊戲知道，它不再需要在定期的遊戲迴圈中繪製該物件。一種方法是在情況發生時，我們可以標記遊戲物件為*死亡*，例如：

```javascript
// 碰撞發生
enemy.dead = true
```

接著，你在重新繪製畫面前，排除掉這些*死亡*的物件，例如：

```javascript
gameObjects = gameObject.filter(go => !go.dead);
```

## 我們該如何發射雷射

發射雷射可以被視作回應一件鍵盤事件，並建立往特定方向移動的物件。因此我們需要列出下列步驟：

1. **建立雷射物件**：從英雄艦艇的正上方，建立往畫面上方移動的物件。
2. **連接該程式到鍵盤事件**：我們需要在鍵盤中挑選一個按鍵，表達玩家發射雷射光。
3. 在按鍵按壓時，**建立看起來像雷射光的遊戲物件**。

## 雷射的冷卻時間

在每次按壓按鍵時，好比說*空白鍵*，雷射光都需要被發射出來。為了讓遊戲不要在短時間內發射太多組雷射光，我們需要修正它。修法為建立*冷卻時間* ── 一個計時器確保雷射在一定期間內只能被發射一次。你可以藉由下列方式建立：

```javascript
class Cooldown {
  constructor(time) {
    this.cool = false;
    setTimeout(() => {
      this.cool = true;
    }, time)
  }
}

class Weapon {
  constructor {
  }
  fire() {
    if (!this.cooldown || this.cooldown.cool) {
      // 產生雷射光
      this.cooldown = new Cooldown(500);
    } else {
      // 什麼事都不做，冷卻中。
    }
  }
}
```

✅ 根據太空遊戲系列課程的第一章，回想關於*冷卻時間*。

## 建立目標

你會利用上一堂課中現成的程式碼(你應該有整理並重構過)做延伸。使用來自 Part II 的檔案或是使用 [Part III - Starter](../your-work)。

> 要點：你需要使用的雷射光已經在資料夾中，並已匯入到程式碼中。

- **加入碰撞偵測**，建立下列規則給各個雷射碰觸到東西的情況：
   1. **雷射擊中敵人**：被雷射擊中的敵人會死亡
   2. **雷射擊中畫面最上方**：雷射擊中到畫面最上方會消失
   3. **敵人碰觸到英雄**：敵人與英雄在互相碰觸時皆會被摧毀
   4. **敵人碰觸到畫面最下方**：敵人碰觸到畫面最下方時，該敵人與英雄會被摧毀
   
## 建議步驟

在你的 `your-work` 子資料夾中，確認檔案是否建立完成。它應該包括：

```bash
-| assets
  -| enemyShip.png
  -| player.png
  -| laserRed.png
-| index.html
-| app.js
-| package.json
```

開始 `your_work` 資料夾中的專案，輸入：

```bash
cd your-work
npm start
```

這會啟動 HTTP 伺服器並發布網址 `http://localhost:5000`。開啟瀏覽器並輸入該網址，現在它能呈現英雄以及所有的敵人，但它們還沒辦法移動！

### 建立程式碼

1. **設定表達遊戲物件為矩形的方法，以處理碰撞狀況** 下列的程式表達 `GameObject` 的矩形呈現方式。編輯你的 GameObject class：

    ```javascript
    rectFromGameObject() {
        return {
          top: this.y,
          left: this.x,
          bottom: this.y + this.height,
          right: this.x + this.width,
        };
      }
    ```

2. **加入程式碼來檢查碰撞** 這會是新函式來測試兩矩形是否相交：

    ```javascript
    function intersectRect(r1, r2) {
      return !(
        r2.left > r1.right ||
        r2.right < r1.left ||
        r2.top > r1.bottom ||
        r2.bottom < r1.top
      );
    }
    ```

3. **加入雷射發射功能**
   1. **加入鍵盤事件訊息**。 *空白鍵*要能在英雄艦艇上方建立雷射光。加入三個常數到 Messages 物件中：

       ```javascript
        KEY_EVENT_SPACE: "KEY_EVENT_SPACE",
        COLLISION_ENEMY_LASER: "COLLISION_ENEMY_LASER",
        COLLISION_ENEMY_HERO: "COLLISION_ENEMY_HERO",
       ```

   1. **處理空白鍵**。 編輯 `window.addEventListener` 的 keyup 函式來處理空白鍵：

      ```javascript
        } else if(evt.keyCode === 32) {
          eventEmitter.emit(Messages.KEY_EVENT_SPACE);
        }
      ```

    1. **加入監聽者**。 編輯函式 `initGame()` 來確保英雄可以在空白鍵按壓時，發射雷射光：

       ```javascript
       eventEmitter.on(Messages.KEY_EVENT_SPACE, () => {
        if (hero.canFire()) {
          hero.fire();
        }
       ```

       建立新的函式 `eventEmitter.on()` 確保敵人碰觸到雷射光時，能更新死亡狀態：

          ```javascript
          eventEmitter.on(Messages.COLLISION_ENEMY_LASER, (_, { first, second }) => {
            first.dead = true;
            second.dead = true;
          })
          ```

   1. **移動物件**。 確保雷射逐步地向畫面上方移動。建立新的 class Laser 延伸自 `GameObject`，你應該有做過：
   
      ```javascript
        class Laser extends GameObject {
        constructor(x, y) {
          super(x,y);
          (this.width = 9), (this.height = 33);
          this.type = 'Laser';
          this.img = laserImg;
          let id = setInterval(() => {
            if (this.y > 0) {
              this.y -= 15;
            } else {
              this.dead = true;
              clearInterval(id);
            }
          }, 100)
        }
      }
      ```

   1. **處理碰撞**。 建立雷射的碰撞規則。加入函式 `updateGameObjects()` 來確認被碰撞的物件。

      ```javascript
      function updateGameObjects() {
        const enemies = gameObjects.filter(go => go.type === 'Enemy');
        const lasers = gameObjects.filter((go) => go.type === "Laser");
      // 雷射擊中某物
        lasers.forEach((l) => {
          enemies.forEach((m) => {
            if (intersectRect(l.rectFromGameObject(), m.rectFromGameObject())) {
            eventEmitter.emit(Messages.COLLISION_ENEMY_LASER, {
              first: l,
              second: m,
            });
          }
         });
      });

        gameObjects = gameObjects.filter(go => !go.dead);
      }  
      ```

      記得在 `window.onload` 裡的遊戲迴圈中加入 `updateGameObjects()`。

   4. **設定雷射的冷卻時間**，它只能在定期內發射一次。

      最後，編輯 Hero class 來允許冷卻：

       ```javascript
      class Hero extends GameObject {
        constructor(x, y) {
          super(x, y);
          (this.width = 99), (this.height = 75);
          this.type = "Hero";
          this.speed = { x: 0, y: 0 };
          this.cooldown = 0;
        }
        fire() {
          gameObjects.push(new Laser(this.x + 45, this.y - 10));
          this.cooldown = 500;
    
          let id = setInterval(() => {
            if (this.cooldown > 0) {
              this.cooldown -= 100;
            } else {
              clearInterval(id);
            }
          }, 200);
        }
        canFire() {
          return this.cooldown === 0;
        }
      }
      ```

到這裡，你的遊戲有了些功能！你可以測試方向鍵，使用空白鍵發射雷射。當你擊中敵人時它們會消失。幹得漂亮！

---

## 🚀 挑戰

加入爆炸特效！ 看看 [Space Art Repo](../solution/spaceArt/readme.txt) 中的檔案，試著在雷射擊中外星人時，加入爆炸畫面。

## 課後測驗

[課後測驗](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/36?loc=zh_tw)

## 複習與自學

對遊戲中的迴圈定時做點實驗。當你改變數值時，發生了什麼事？閱讀更多關於 [JavaScript 計時事件](https://www.freecodecamp.org/news/javascript-timing-events-settimeout-and-setinterval/)。

## 作業

[探索碰撞](assignment.zh-tw.md)
