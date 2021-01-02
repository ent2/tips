# tips
엔틜 노팁에 썼던 글들 백업

## 말하기 개행 관련 팁
`2019/01/01`  
http://naver.me/FECGB8de
말하기 블록으로 띄운 말풍선의 한 줄에 표시되는 글자수가 15개라고 합니다. 
위 작품은 그 점을 이용하여 \n(개행 부호)를 입력하면 줄이 바뀌도록(개행) 만들었습니다.
이 외에도 아스키 아트 등을 만드는데 응용할 수 있겠습니다.

## 소리가 얼마나 다운로드 되었는지 확인하기
`2019/11/06`  
```js
var t = setInterval(()=>{
var c=0;
var e=Entry.soundQueue._loadQueueBackup;
e.forEach(o => o.loaded?(()=>c++)():0);
e.length!=c?console.log(`${c} / ${e.length}`):(()=>{console.log("로딩 완료");clearInterval(t)})();
}, 1000);
```

위 코드를 복사한 뒤, 원하는 작품 창에서 Ctrl + Shift + J 를 눌러 Console 창을 열고 붙여넣고, 엔터 키를 누르세요.

이 코드는 
https://github.com/entrylabs/entryjs/pull/1506/commits/63d06de0e77e7bceaf55a46f5cf41253556bb320
을 참고하여 작성하였습니다.
~~(어차피 한 달 안에 업데이트 되어 적용될테니 그 전까지만 사용하시면 되겠습니다.)~~

## 테이블을 이용해 `~번째 항목 변경/조회가 가능한 리스트` 만들기
`2021/01/01`  
![1](https://playentry.org/uploads/discuss/1i/hr/image/1ihrf73skjeafq590iysbe352ffavcwo.png)
![2](https://playentry.org/uploads/discuss/dn/vw/image/dnvwvj6kkjeaftq835bvbe352f4nl9vd.png)

리스트의 최대 단점은 없는 항목에 바로 접근할 수가 없다는거죠. (첫번째 사진의 코드는 에러가 발생합니다)
하지만 테이블을 사용하면 임의의 항목에 정보를 입력할수가 있습니당// (두번째 사진)

항목 번호는 원하는 숫자를 넣으시면 됩니다. (심지어 소수점 아래 까지 구별합니다.)
문자까지 지원했으면 더 좋았을 탠데 아쉽네요.
(문자도 넣을 수 있긴 한데, 어떤 문자를 넣든 문자 끼리는 똑같은 항목으로 치더라고요.)

노팁에 이미 있을 줄 알았는데 의외로 찾아도 안나오네요.

## 가상 오브젝트 패턴: "다른 오브젝트"의 위치 변경하기
`2021/01/03`  
![1](https://playentry.org/uploads/discuss/ur/04/image/ur04jhs0kjfud1xs0olfbe352fc633m1.png)
![2](https://playentry.org/uploads/discuss/xh/uh/image/xhuhalaskjfudefj14bfbe352fg1fkxz.png)
![3](https://playentry.org/uploads/discuss/0l/ys/image/0lys9slpkjfudjbb14kjbe352f282tjh.png)
![4](https://playentry.org/uploads/discuss/0j/9l/image/0j9l5gyfkjfudni42772be352fbbdwvk.png)

엔트리의 기본 위치 이동, 크기 변경, 말하기 블록 등은 현재 오브젝트에만 적용할 수 있어,
여러 오브젝트를 동시에 조작하기 불편하다는 단점이 있습니다.

이를 해결하기 위해 테이블에 좌표 데이터를 저장하고,
각 오브젝트는 그 정보를 받아 움직이기만 하도록 할 수 있습니다.

예제 작품: http://naver.me/5bX84VM1

가상 오브젝트 패턴은 크게 두 함수; assign(지정하다)과 bind(묶다)로 이루어집니다.
그리고 이동 방향을 오브젝트의 ID로 사용합니다. (https://playentry.org/ds#!/tips/5fe30a76d683230f99472d7d 참고)

- assign (첫번째 사진) 함수는 테이블에 원하는 값을 정하는 함수입니다,
 입력값으로 '오브젝트의 ID(이동 방향)', '바꿀 항목(x 또는 y)', '바꿀 값(좌표값)'을 받습니다.

 (이때 주의할 점은, '바꿀 항목'번째 행, 'ID' 열에 데이터를 저장한다는 것입니다.
 엔트리의 테이블은 아무 "열"에는 접근할 수 있지만, "행"은 이미 추가되어 있을 때만 접근할 수 있기 때문에, 
 오브젝트의 ID를 자유롭게 사용하기 위해 이를 "열" 번호로 정했습니다.
 https://playentry.org/ds#!/tips/5fef1f01d1d2ad007b9b15e4 참고)

- bind (두번째 사진) 함수는 테이블에서 데이터를 불러와 현재 오브젝트에 적용하는 함수입니다.
 입력값으로 '오브젝트의 ID', 'x좌표 기본값', 'y좌표 기본값'을 받습니다.
 오브젝트의 ID(이동 방향)을 정하고,
 데이터의 기본값을 정하고,
 계속 데이터값을 받아 오브젝트를 이동시킵니다.
 (bindCount 변수는 모든 오브젝트에서 bind가 끝났는지 확인하기 위해 사용합니다.)

더 자세한 작동 원리는 예제 작품의 코드를 참고해주세요.

.....
가상 오브젝트라는 이름은 비슷한 방식의 "가상 DOM" 이라는 웹 기술의 이름에서 따오긴 했지만,
웹페이지의 성능을 향상시키기 위한 "가상 DOM"과 달리,
가상 오브젝트 패턴은 코딩을 쉽게하기 위한 목적이 큽니다. :D
bind 함수에서 계속 반복하기를 이용해 이동하기 때문에
오히려 성능은 떨어질 수도 있어요. :(
더 좋은 이름이 있으면 알려주세요!
