# Space 게임 제작하기 파트 4: 레이저 추가하고 충돌 감지하기

## 강의 전 퀴즈

[Pre-lecture quiz](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/35?loc=ko)

이 강의에서는 JavaScript로 레이저를 쏘는 방법을 배웁니다! 게임에 다음 두 가지를 추가합니다:

- **레이저**: 이 레이저는 영웅 우주선에서 수직 위쪽으로 발사되며
- **충돌 감지**, *쏘는* 기능 구현의 부분으로 몇 가지 멋진 게임 규칙을 추가할 예정입니다:
   - **레이저로 적 때리기**: 레이저에 맞으면 적은 사망합니다
   - **레이저로 화면 상단 도달하기**: 화면의 상단 부분을 맞으면 레이저는 파괴됩니다
   - **적과 영웅 충돌하기**: 적과 영웅이 부딪히면 파괴됩니다
   - **적이 화면 하단 도달하기**: 적이 화면 하단에 부딪히면 적과 영웅이 파괴됩니다

짧게 요약해보면, 여러분은 -- *영웅* -- 모든 적들이 화면 아래로 내려오기 전에 레이저로 모든 적을 때려야 합니다.

✅ 지금까지 작성된 최초의 컴퓨터 게임에 대해 약간 알아보새요. 어떻게 작동하나요?

함께 영웅이 됩시다!

## 충돌 감지하기

충돌은 어떻게 감지할까요? 게임 객체를 움직이는 직사각형으로 생각해야 합니다. 왜 물어볼까요? 게임 객체를 그리는 데 사용되는 이미지는 직사각형이기 때문입니다: `x`, `y`, `width` 그리고 `height`가 있습니다.

만약 두 직사각형이, 즉 영웅과 적이 *교차*하면, 충돌합니다. 그런 뒤에 발생할 일은 게임의 룰에 달려 있습니다. 따라서 충돌 감지를 구현하려면 다음이 필요합니다:

1. 게임 객체의 직사각형 표현을 얻는 방법은, 다음과 같습니다:

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

2. 비교 함수는, 다음 함수와 같습니다:

   ```javascript
   function intersectRect(r1, r2) {
     return !(r2.left > r1.right ||
       r2.right < r1.left ||
       r2.top > r1.bottom ||
       r2.bottom < r1.top);
   }
   ```

## 어떻게 파괴할까요

게임에서 물건을 파괴하려면 특정 간격으로 연결되는 게임 루프에서 이 아이템을 더 이상 그리지 않아야 한다고 게임에 알려야 합니다. 이 방법은 다음과 같이, 어떤 일이 발생했을 때 게임 객체를 *dead*으로 표시합니다:

```javascript
// collision happened
enemy.dead = true
```

그러고 다음과 같이, 화면을 다시 그리기 전에 *dead* 객체를 정렬합니다:

```javascript
gameObjects = gameObject.filter(go => !go.dead);
```

## 어떻게 레이저를 발사할까요

레이저를 발사하는 것은 키-이벤트에 반응하고 특정 방향으로 움직이는 객체를 만드는 것으로 바뀝니다. 따라서 다음 단계를 해야합니다:

1. **레이저 객체 생성**: 영웅 함선의 상단에서, 생성되고 화면 상단으로 올라가기 시작합니다.
2. **키 이벤트에 코드 첨부**: 레이저를 쏘는 플레이어로 특정할 키보드의 키를 선택해야 합니다.
3. 키를 누를 때, **레이저처럼 보이는 게임 객체를 생성합니다**.

## 레이저 쿨다운

레이저는 *space*와 같은 키를 누를 때마다 발사되어야 합니다. 게임이 짧은 시간에 너무 많은 레이저를 생성하는 것을 막으려면 이를 해결해야 합니다. 해결 방법은 레이저를 자주 발사하도록 보장하는 타이머인, *cooldown*을 구현하는 것입니다. 다음과 같이 구현할 수 있습니다:

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
      // produce a laser
      this.cooldown = new Cooldown(500);
    } else {
      // do nothing - it hasn't cooled down yet.
    }
  }
}
```

✅ *cooldowns*에 대해 복습하려면 space 게임 시리즈의 1강을 참조하세요.

## 무엇을 만드나요

이전 강의에 존재한 기존 코드 (정리하고 리팩토링함)를 가져와서, 확장합니다. 파트 II에서 코드를 시작하거나 [Part III- starter](../your-work) 코드를 사용합니다.

> tip: 작업할 레이저는 이미 어셋 폴더에 있으므로 코드에서 참조합니다

- **충돌 감지를 추가합니다**, 레이저가 무언가 부딪칠 때 다음 규칙이 적용되어야 합니다:
   1. **레이저가 적 때리기**: 레이저에 맞으면 적은 사망합니다
   2. **레이저로 화면 상단 도달하기**: 화면의 상단 부분을 맞으면 레이저는 부서집니다
   3. **적과 영웅 충돌하기**: 적과 영웅이 부딪히면 파괴됩니다
   4. **적이 화면 하단 도달하기**: 적이 화면 하단에 부딪히면 적과 영웅이 파괴됩니다

## 권장 단계

`your-work` 하위 폴더에 생성된 파일을 찾습니다. 이는 다음을 포함하고 있어야 합니다:

```bash
-| assets
  -| enemyShip.png
  -| player.png
  -| laserRed.png
-| index.html
-| app.js
-| package.json
```

타이핑하여 `your_work` 폴더에서 프로젝트를 시작합니다:

```bash
cd your-work
npm start
```

위 코드는 `http://localhost:5000` 주소에서 HTTP 서버를 시작합니다. 브라우저를 열고 해당 주소를 입력하세요, 지금 바로 영웅과 모든 적을 렌더링해야합니다, 하지만 다 움직이지 않습니다 - 아직 :).

### 코드 추가하기

1. ***충돌을 처리하기 위해 게임 객체의 사각형 표현을 설정합니다** 아래 코드를 쓰면 `GameObject`의 사각형 표현을 얻을 수 있습니다. GameObject 클래스를 편집하여 확장합니다:

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

2. **충돌을 확인하는 코드를 추가합니다** 이것은 두 개의 직사각형이 교차되는가에 대한 여부를 테스트하는 새로운 함수입니다:

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

3. **레이저 발사 기능 추가**
   1. **키-이벤트 메시지 추가하기**. *space* 키는 영웅 함선 바로 위에 레이저를 만들어줘야 합니다. Messages 객체에 세 개의 상수를 추가합니다:

       ```javascript
        KEY_EVENT_SPACE: "KEY_EVENT_SPACE",
        COLLISION_ENEMY_LASER: "COLLISION_ENEMY_LASER",
        COLLISION_ENEMY_HERO: "COLLISION_ENEMY_HERO",
       ```

   1. **space 키 제어하기**. `window.addEventListener` 키업 함수로 spaces를 제어합니다:

      ```javascript
        } else if(evt.keyCode === 32) {
          eventEmitter.emit(Messages.KEY_EVENT_SPACE);
        }
      ```

    1. **리스너 추가하기**. `initGame()` 함수를 편집해서 space 바를 눌렀을 때 hero가 발사할 수 있도록 합니다:

       ```javascript
       eventEmitter.on(Messages.KEY_EVENT_SPACE, () => {
        if (hero.canFire()) {
          hero.fire();
        }
       ```

       새로운 `eventEmitter.on ()` 함수를 추가해서 적이 레이저와 부딪칠 때 동작하도록 합니다:

          ```javascript
          eventEmitter.on(Messages.COLLISION_ENEMY_LASER, (_, { first, second }) => {
            first.dead = true;
            second.dead = true;
          })
          ```

   1. **객체 움직이기**, 레이저가 화면 상단으로 조금씩 이동하고 있는지 확인합니다. 저번처럼, `GameObject`를 확장하는 새로운 Laser 클래스를 만듭니다:
   
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

   1. **충돌 제어하기**, 레이저에 대한 충돌 규칙을 구현합니다. 적에 충돌하는 객체를 테스트하는 `updateGameObjects ()` 함수를 추가합니다:

      ```javascript
      function updateGameObjects() {
        const enemies = gameObjects.filter(go => go.type === 'Enemy');
        const lasers = gameObjects.filter((go) => go.type === "Laser");
      // laser hit something
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

      `window.onload`의 게임 루프에 `updateGameObjects()`를 추가해야 합니다.

   4. 레이저의 **cooldown을 구현합니다**, 그래서 자주 발사할 수 있습니다.

      마지막으로, cooldown을 할 수 있도록 Hero 클래스를 편집합니다:

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

여기에서 핵심은, 게임이 몇 가지 기능을 가지고 있다는 사실입니다! 화살표 키로 탐색하고, 스페이스 바로 레이저를 발사할 수 있으며, 적을 치면 사라지게 합니다. 잘 하셨습니다!

---

## 🚀 도전

폭발을 추가합니다! [the Space Art repo](../../solution/spaceArt/readme.txt)에서 게임 어셋을 살펴보고 레이저가 외계인을 칠 때 폭발하도록 추가해보세요

## 강의 후 퀴즈

[Post-lecture quiz](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/36?loc=ko)

## 리뷰 & 자기주도 학습

지금까지 게임의 간격을 실험 해보세요. 바꾸면 어떻게 되나요? [JavaScript timing events](https://www.freecodecamp.org/news/javascript-timing-events-settimeout-and-setinterval/)에 대하여 더 읽어보시기 바랍니다.

## 과제

[Explore collisions](../assignment.md)
