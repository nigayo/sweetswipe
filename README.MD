SweetSwipe
============
A lightweight swiping(flicking) mobile web component.

<br>

-------
## Demo

* <a href="https://skplanet.github.io/sweetswipe/demo/swipe.html" target="_blank">Base</a>
* <a href="https://skplanet.github.io/sweetswipe/demo/swipe_circular_stepmover.html" target="_blank">Circular</a>
* <a href="https://skplanet.github.io/sweetswipe/demo/swipe_circular_responsive_width.html" target="_blank">Circular responsive width</a>

<img src="demo/img/swipedemo.png" width="300">

<br>

-------
## Feature

* lightweight (vanillaJS)
* CSS3 based fast animation
* circular sliding

<br>

-------
## Compatibility

| Device | Browser | 
| ------ | ------- |
| Desktop| IE9+, FF, Chrome, Safari |
| Mobile | Android 4.x, Chrome, Safari |

<br>

-------
## Install

* npm install
* gulp buildJS (ES6 -> ES5)
* import script
```HTML
<script src="../dist/swipe_merge_es5.js"></script>
```

-------
## Install using Bower

* bower install sweetswipe

<br>

-------
## Usage - HTML 

```HTML
   <div class="viewArea">
        <div id="swipeWrap">
            <div>menu1-list</div>
            <div>menu2-list </div>
            <div>menu3-list </div>
        </div>
    </div>
```

-------
## Usage - CSS 
```CSS
    .viewArea {
        overflow: hidden;
        height: 200px;
    }

    #swipeWrap {
        position : relative;
        width :100%;
        -webkit-transform: translate3d(0px, 0px, 0px);
        transform: translate3d(0px, 0px, 0px);
    }

    #swipeWrap div {
        width :100%;
        position: absolute;
        top: 0;
        height: 200px;
    }

    #swipeWrap div:nth-child(1) {
        transform: translate3d(0%, 0px, 0px);
        -webkit-transform: translate3d(0%, 0px, 0px);
        background-color: red;
    }
    #swipeWrap div:nth-child(2){
        background-color: green;
        transform: translate3d(100%, 0px, 0px);
        -webkit-transform: translate3d(100%, 0px, 0px);
    }
    #swipeWrap div:nth-child(3){
        transform: translate3d(200%, 0px, 0px);
        -webkit-transform: translate3d(200%, 0px, 0px);
        background-color: blue;
    }

```

<br>

-------
## Usage - JavaScript

```javascript

  var oMyswipe = new SweetSwipe($("#swipeWrap"), {
      'bCircular': true,
      'nDuration': 100,
      'nBackWidth': 60,
      'nSideWidth': 20,
      'nDecisionSlope': 0.8,
      'nForcedSwipeTime': 100,
      'bSettingScreenHeight': false,
      'bMouseEventSupport': true,
      'bTouchEventSupport' : false,
    });

```

<br>

-------
## Options 

| Option | Description|
| ------ | -----------|
| bCircular | 무한롤링을 할지 결정 |
| nDuration | 스와이프 애니메이션 소요시간 |
| nBackWidth | 지정된px을 넘으면 스와이핑이 일어남. 그 이하면 스와이핑이 안되고 돌아옴 |
| nSideWidth | 무한롤링이 아닐때 처음과 마지막 패널이 살짝 가는 척하는 거리(px) |
| nDecisionSlope | 스와이핑이 일어날 수 있는 touchmove의 기울기 |
| nForceSwipeTime | 터치이동거리에 상관없이 스와이핑이 일어날 수 있는 시간(millisecond)|
| bSettingScreenHeight | 스와이핑이 끝날때마다 패널의 높이값을 계산해서 높이를 지정해줘야 할 때 사용 |
| bMouseEventSupport | 마우스 이벤트에 스와이핑 기능이 필요하다면 true |
| bTouchEventSupport | 터치나 마우스 기반의 스와이핑이 필요하지 않다면 false |


<img src="demo/img/swipe.png" width="350">

<br>

-------
## Custom function
You can register custom function that executed at specific point of time.

| Custom function | Description | Parameter |
| --------------- | ----------- | --------- |
| FN_BEFORE_SWIPE | Before swiping | current panel order (0~n)|
| FN_AFTER_SWIPE | After swiping | current panel order (0~n)|
| FN_COMPONENT_DID_LOAD | After component initialization ||


#### Usage
```javascript
    oMyswipe.registerUserMethod({
      'FN_BEFORE_SWIPE': function(n){},
      'FN_AFTER_SWIPE': function(n){},
      'FN_COMPONENT_DID_LOAD' : function(){console.log("end of component init");}
    });
```

<br>

-------
## API
| Function | Description | return | Parameter |
| -------- | ----------- | ------ | --------- |
| getCurrentViewNumber() | Get panel order number | Number(0~n) : panel order | |
| runSwipeAction(n,n) | Move to specific panel | | Number(millisecond) : anmation duration , Number : panel order to move(0~n) |


<br>

-------
## Usage of Circular swiping(무한 슬라이딩)

##### JavaScript
아래와 같이 bCircular만 true로 두면 된다.
```javascript
   'bCircular': true,
```

##### HTML
무한롤링을 구현하기 위해서는 맨앞과 맨뒤에 복제한 노드를 추가해야 한다.
예를들어 1,2,3,4이렇게 4개의 패널(노드)가 있다면, 앞에 4, 뒤에 1번 패널을 복제해서 추가한다.
javascript로 불필요하게 복제하지 않고, HTML을 미리 구현하는 것이 성능에 좀더 유리하다.
따라서 실제 HTML구조는 "1234 -> 412341" 가 될 것이다.

아래 참고 코드를 살펴보자
: [go to demo page]( https://github.com/skplanet/sweetswipe/blob/master/demo/dev_circular_app.html)


##### CSS

```CSS
 #swipeWrap > div:nth-child(1) {
                -webkit-transform: translate3d(-100%, 0px, 0px);
            }
            #swipeWrap > div:nth-child(2){
                -webkit-transform: translate3d(0%, 0px, 0px);
            }
            #swipeWrap > div:nth-child(3){
                -webkit-transform: translate3d(100%, 0px, 0px);
            }

            #swipeWrap > div:nth-child(4){
                -webkit-transform: translate3d(200%, 0px, 0px);
            }

            #swipeWrap > div:nth-child(5){
                -webkit-transform: translate3d(300%, 0px, 0px);
            }

            #swipeWrap > div:nth-child(6){
                -webkit-transform: translate3d(400%, 0px, 0px);
            }
```

<br>

------
## Usage of auto swiping(자동 슬라이딩)

아래와 같이 사용하면 자동으로 swiping 된다. 
참고코드 


##### JavaScript
```javascript
  (function movePanel() {
    setTimeout(function(){
      oMyswipe.runAutoAnimation("toRight");
      movePanel();
    }, 5000);
  })();
```

참고코드 : 
* <a href="https://skplanet.github.io/sweetswipe/demo/swipe_circular_responsive_width.html" target="_blank">Circular responsive width</a>

-------
##  Plugin - SwipeNavigationPlugin
몇 개 Plugin으로 확장기능을 사용할 수 있다.

Tab menu navigation UI.

<img src="demo/img/tabmenu.png" width="250">

```HTML
  <ul>
    <li class="selected-swipe-li"><a href="#">first</a></li>
    <li class="mockclass"><a href="#">two</a></li>
    <li><a href="#">third</a></li>
    <li><a href="#">fourth</a></li>
  </ul>
```

```javascript
  oMyswipe.onPlugins([
    { 
     'name': 'SwipeNavigationPlugin',
     'option': {
       'usage': true,
       'elNaviWrap': document.querySelector("#swipeNaviWrap > ul"),
       'seletedClassName': 'selected-swipe-li',  // CSS class name of selected element.
       'nDuration': 200,   //swipe animation duration(millisecond).
       'bMouseEventSupport': true  //click event support
      },
      'userMethod' : {}
    }
  ]);
```

<br>

##  Plugin - SwipeStepMoverPlugin
Button menu navigation UI

<img src="demo/img/button.png" width="250">


```HTML
  <div class="buttonNavi">
    <button class="swipe-prev"><</button>
    <button class="swipe-next">></button>
  </div>
```


```javascript
  oMyswipe.onPlugins([
    'name'      : 'SwipeStepMoverPlugin',
    'option'    : {
      'prevButton' : document.querySelector(".swipe-prev"),
      'nextButton' : document.querySelector(".swipe-next"),
      'nDuration': 200
    },
    'userMethod' : {}
  ]);
```

<br>

## Test
<a href="https://skplanet.github.io/sweetswipe/spec/swipe.test.html" target="_blank">unit test page</a>

<br>

## Contributing
See our [CONTRIBUTING.md](https://github.com/skplanet/sweetswipe/blob/master/CONTRIBUTING.md) for information on how to contribute.

<br>

## License
MIT Licensed. Copyright (c) 2016 SK PLANET. All Rights Reserved.


