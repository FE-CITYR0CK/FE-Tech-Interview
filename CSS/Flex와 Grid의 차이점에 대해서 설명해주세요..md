# Flex와 Grid의 차이점에 대해서 설명해주세요.

<br/>

## 면접용

Flex와 Grid는 각각 다른 레이아웃 목적에 최적화된 CSS 도구입니다.

Flex는 **1차원 레이아웃**으로 한 축(가로 또는 세로)만 제어 가능하며 주로 수평 방향으로 레이아웃을 정렬합니다. Grid는 **2차원 레이아웃**으로 행(row)과 열(column)을 동시에 제어합니다. Flex는 콘텐츠 중심으로 유동적인 아이템 정렬(`justify-content`, `align-items`)에 적합합니다. 반면에, Grid는 레이아웃 중심으로  아이템크기를 미리 정의하고 셀의 크기를 일정하게 유지합니다. 즉, 고정된 구조 설계(`grid-template-rows`, `columns`)에 적합합니다. Flex는 네비게이션 바나 버튼 그룹처럼 단순 정렬이 필요할 때 유리하며, Grid는 대시보드나 카드형 UI처럼 정교한 2차원 배치가 필요한 경우 적합합니다.

<br/>
<hr/>
<br/>

## 개념설명

### **1. Flexbox란?**

- **한 방향 레이아웃 시스템을 의미합니다.**
    - **아이템 배치**: **가로(row)** 또는 **세로(column)** 방향으로 배치합니다,
    - 아이템 배치: 일차원적 배치합니다,
    - x,y 동시에 제어하지 못함: 둘 중 하나를 주축으로 요소 배치합니다.
    - 메인축을 바탕으로 정렬되게됩니다.
    ![Image](https://github.com/user-attachments/assets/bd43a1e7-790f-44ec-a3a1-c8c8c8dd11c7)
    ![Image](https://github.com/user-attachments/assets/adce282f-4d33-4aa2-bfe8-6cd29409ee8a)
    
- **주요 개념**
    - `display: flex;`: 부모 컨테이너를 Flexbox로 선언합니다.
    - 기본 속성: **row** (가로 방향)
    - 가로 선상: `justify-content` → 아이템의 **가로 정렬**합니다.
    - 세로 선상: `align-items` → 아이템의 **세로 정렬**합니다.
    - `flex-direction`: 정렬할 **방향 지정**합니다.
        - `row`(기본값): 왼→오 가로 방향으로 정렬합니다.
        - `column`: 위→아래 세로 방향으로 정렬합니다.
        - `row-reverse` / `column-reverse`: 반대 방향으로 정렬합니다.

### **Flexbox 주요 속성**

- **중심 축 정렬**: `justify-content`
    - Flex Item을 중심축 방향(Main Axis)으로 정렬합니다
    - `flex-start`: 왼쪽으로 정렬 (기본값)합니다.
    - `flex-end`: 오른쪽으로 정렬합니다.
    - `center`: 가운데 정렬합니다.
    - `space-between`: 요소 간 **균등한 간격** 배치합니다.
    - `space-around`: 양쪽 여백 포함하여 간격 배치합니다.
    
- **교차축 정렬**: `align-items`
    - Flex Item을 교차축 방향(Cross Axis)으로 정렬합니다.
    - `stretch`: 컨테이너 크기에 맞게 늘어납니다.(기본값)
    - `flex-start`: 위쪽으로 정렬합니다.
    - `flex-end`: 아래쪽으로 정렬합니다.
    - `center`: 교차축 기준으로 세로 중앙에 정렬합니다.
    - `baseline`: 요소 내부 텍스트의 기준선을 맞춰 정렬합니다.

```css

.container {
  display: flex; /* Flexbox 레이아웃 설정*/
  flex-direction: row; /* 가로 정렬 */
  justify-content: center; /* 가로 축 가운데 정렬 */
  align-items: center; /* 세로 축 가운데 정렬 */
}

```

---

### **2. Grid란?**

- **두 방향레이아웃 시스템을 의미합니다.**
    - **두 방향 레이아웃 시스템 (가로, 세로)입니다.**
    - 아이템 배치: 이차원으로 배치합니다.
    
   ![Image](https://github.com/user-attachments/assets/1581a761-24a3-43a8-8669-d0c3c14dfd10)
    

### **Grid 주요 개념**

- `display: grid;`: 부모 컨테이너를 Grid로 선언합니다.
- **Grid Template 설정**
    - `grid-template-columns` / `grid-template-rows`: 열과 행의 개수 및 크기를 설정합니다.
    - `gap`: 셀 사이 간격 설정합니다.
- **반응형 크기 설정**
    - `repeat()`: 반복적인 크기를 설정합니다.
    - `minmax()`: 최소/최대 크기 지정 가능합니다.

### **Grid 코드 예제**

```css

.container {
  display: grid;
  /* 3개의 고정 열 */
  grid-template-columns: 100px 100px 100px; 
  /* 행 크기 반복 설정 */
  grid-template-rows: 100px repeat(2, 150px); 
  /* 셀 사이 간격 */
  gap: 10px;
  }
```

- **셀 크기 비율 지정**

```css
/* 부모 컨테이너를 3등분 */
grid-template-columns: 1fr 1fr 1fr;
```

- 만약 2번에서 4번까지 영역을 하나로 하고싶다고 가정한다면?

```css
.item2{
 grid-column-start:2;
 grid-column-end:4;
  /* 이렇게 축약해서 표현도 가능함! */
 grid-column: 2 / 4; 
 
 
 /*세로인 부분 차지*/
 grid-row-start:2;
 grid-row-end:4;
```

### **아이템 배치 예제**

```css

.item {
  grid-column: 2 / 4; /* 2번에서 4번 열까지 차지 */
  grid-row: 1 / 3; /* 1번에서 3번 행까지 차지 */
}

```



### **3. Flexbox와 Grid의 차이점**

| **항목** | **Flexbox** | **Grid** |
| --- | --- | --- |
| **레이아웃 방식** | 1차원 (가로 or 세로) | 2차원 (가로 + 세로) |
| **적합한 경우** | 단순한 콘텐츠 배치, 유연한 아이템 정렬 | 정교한 레이아웃 설계, 행/열 배치 |
| **정렬 기준** | 주축을 기준으로 아이템 정렬 | 전체 그리드를 템플릿으로 제어 |

### **4. 언제 Flexbox와 Grid를 사용할까요?**

- **Flexbox를 사용하는 경우**
    - 메뉴, 버튼 정렬처럼 단순한 **한 줄 정렬**이 필요할 때 사용합니다.
    - 아이템 간의 **유연한 공간 분배**가 중요할 때 사용합니다,
- **Grid를 사용하는 경우**
    - **전체 페이지 레이아웃**이나 복잡한 **레이아웃**을 설계할 때 사용합니다.
    - 정확히 **행/열 기준으로 아이템을 배치**해야 할 때 사용합니다.