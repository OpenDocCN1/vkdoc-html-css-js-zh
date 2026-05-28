# 4. 处理输入和事件

## 4.1 引言

处理用户输入是任何游戏的重要组成部分。在 Cocos2d-js 中，用户输入（例如通过触摸、鼠标或键盘）以事件的形式提供。事件机制涉及三个主要部分（见图 4-1）。

![A435521_1_En_4_Fig1_HTML.jpg](img/A435521_1_En_4_Fig1_HTML.jpg)

图 4-1. Cocos2d-JS 中的事件机制

### 4.1.1 事件触发器

事件触发器是任何事件的源头。当发生触摸或任何其他输入时，Cocos2d-js 系统将触发一个事件作为响应。

### 4.1.2 事件管理器

当任何事件被触发时，事件管理器会收到通知。`EventManager` 是一个单例实例，是 Cocos2d-js 系统的一部分；它管理所有事件。

### 4.1.3 事件监听器

这是实际事件处理逻辑所在之处。事件管理器会将事件分派给所有事件监听器，并由事件监听器实例进行处理。

## 4.2 触摸事件

有两种可用的触摸事件：单点触摸（`cc.EventListener.TOUCH_ONE_BY_ONE`）和多点触摸（`cc.EventListener.TOUCH_ALL_AT_ONCE`）。

### 4.2.1 单点触摸

在 `src` 文件夹中，创建一个名为 `touchevents.js` 的文件，并将以下代码复制进去：

```javascript
var TouchEventsLayer = cc.Layer.extend({
    sprite:null,
    ctor:function () {
        this._super();

        var size = cc.winSize;

        this.sprite = new cc.Sprite(res.Sprite_Image);
        this.sprite.attr({
            x:size.width / 2,
            y:size.height / 2
        });
        this.addChild(this.sprite, 0);
        this.sprite.tag='TouchTarget';

        //创建事件监听器对象
        var listener = cc.EventListener.create({
            event: cc.EventListener.TOUCH_ONE_BY_ONE,
            swallowTouches: true,

            onTouchBegan: function (touch, event) {
                var target = event.getCurrentTarget();
                var locationInNode = target.convertToNodeSpace(touch.getLocation());
                var s = target.getContentSize();
                var rect = cc.rect(0, 0, s.width, s.height);

                //检查点击区域
                if (cc.rectContainsPoint(rect, locationInNode)) {
                    cc.log('触摸开始：在精灵内部');
                    //返回 true 以启动 OnTouchMove
                    return true;
                }
                cc.log('触摸开始：在精灵外部');
                return false;
            },
            onTouchMoved: function (touch, event) {
                var target = event.getCurrentTarget();
                target.setPosition(touch.getLocation());
            },
            onTouchEnded: function (touch, event) {
                cc.log('触摸结束');
            }
        });

        //将事件监听器添加到精灵
        cc.eventManager.addListener(listener, this.sprite);
        return true;
    }
});
```

在此示例中，精灵可以通过单点触摸进行拖动。事件监听器使用事件名称 `cc.EventListener.TOUCH_ONE_BY_ONE` 创建，并将 `swallowTouches` 设置为 `true`。当多个事件监听器为同一事件名称附加到同一目标时，第一个 `swallowTouches: true` 的监听器将吞噬该事件，并且该事件将不会传递给下一个监听器。

```javascript
cc.eventManager.addListener(listener, this.sprite);
```

在这种情况下，即使精灵是目标，屏幕上的每一次点击都会触发触摸事件。为了确认触摸是否发生在精灵内部，编写了以下代码：

```javascript
var target = event.getCurrentTarget();
var locationInNode = target.convertToNodeSpace(touch.getLocation());
var s = target.getContentSize();
var rect = cc.rect(0, 0, s.width, s.height);

//检查点击区域
if (cc.rectContainsPoint(rect, locationInNode)) {
    cc.log('触摸开始：在精灵内部');
    //返回 true 以启动 OnTouchMove
    return true;
}
```

在 `onTouchBegan` 中，检查了触摸是否发生在精灵内部的条件。如果触摸在精灵内部，则返回 `true`，以便触摸移动的手势将被定向到 `onTouchMoved` 函数；目标位置会根据触摸移动进行更改。

### 4.2.2 多点触控

多点触控事件监听器可以通过事件名称 `cc.EventListener.TOUCH_ALL_AT_ONCE` 创建。请参考以下代码片段：

```javascript
var listener = cc.EventListener.create({
    event: cc.EventListener.TOUCH_ALL_AT_ONCE,
    swallowTouch: true,
    onTouchesBegan: function (touches, event) {
        .......
    },
    onTouchesMoved: function (touches, event) {
        .......
    },
    onTouchesEnded: function (touches, event) {
        .......
    }
});
```

因此，多点触控的回调与单点触控不同。它包含 `onTouchesBegan`、`onTouchesMoved` 和 `onTouchesEnded` 函数。`touches` 对象将包含每次触摸的详细信息，并带有触摸 ID。通常情况下，多点触控可用于地图缩放、多指操作控制等场景。

## 4.3 鼠标事件

与触控事件类似，鼠标事件通过事件名称 `cc.EventListener.MOUSE` 来引用。在 `src` 文件夹中，创建一个名为 `mouseevents.js` 的文件，并将以下代码复制进去：

```
var MouseEventsLayer = cc.Layer.extend({
    sprite:null,
    ctor:function () {
        this._super();
        var size = cc.winSize;
        this.sprite = new cc.Sprite(res.Sprite_Image);
        this.sprite.attr({
            x: size.width / 2,
            y: size.height / 2
        });
        this.addChild(this.sprite, 0);
        this.sprite.tag='TouchTarget';
        //Creating Event Listener Object
        var listener = cc.EventListener.create({
            event: cc.EventListener.MOUSE,
                swallowTouches: true,
                ismousedown:false,
            onMouseDown: function (event) {
                var target = event.getCurrentTarget();
                var locationInNode = target.convertToNodeSpace(event.getLocation());
                var s = target.getContentSize();
                var rect = cc.rect(0, 0, s.width, s.height);
                            //Check the click area
                if (cc.rectContainsPoint(rect, locationInNode)) {
                    cc.log('Mouse Down: Inside the sprite');
                    this.ismousedown=true;
                }
                cc.log('Mouse Down: Outside the sprite');
                return false;
            },
            onMouseMove: function (event) {
                if(this.ismousedown)
                {
                    var target = event.getCurrentTarget();
                    target.setPosition(event.getLocation());
                }
            },
            onMouseUp: function (event) {
               cc.log('Mouse Up');
               this.ismousedown=false;
            }
        });
        //Added Event Listener To Sprite
        cc.eventManager.addListener(listener, this.sprite);
        return true;
    }
});
```

在这个例子中，精灵同样可以通过鼠标点击来拖动，但触控和鼠标事件处理有一个主要区别。在触控中，`onTouchBegan` 函数必须返回 `true`，才能处理 `onTouchMove`。而在鼠标事件中，`onMouseMove` 不依赖于 `onMouseDown` 的返回值。这两个事件可以同时处理。为了跟踪鼠标是否在精灵内部按下，我们引入了 `ismousedown` 标志。当鼠标按下事件发生在精灵内部时，位置将通过 `onMouseMove` 来改变。

### 4.4 键盘事件

在 `src` 文件夹中，创建一个名为 `keyboardevent.js` 的文件，并将以下代码复制进去：

# Cocos2d-JS 事件处理

在上述代码中，同样的精灵拖动操作是通过上、下、左、右组合键来完成的。系统会检查 `onKeyPress` 函数中的键码，如果键码是任何一个箭头键，精灵的 x、y 坐标就会相应地移动。

```javascript
var KeyboardEventsLayer = cc.Layer.extend({
    sprite:null,
    ctor:function () {
        this._super();
        var size = cc.winSize;
        var KeyCode={
            LEFT:37,
            UP:38,
            RIGHT:39,
            DOWN:40
        };
        var MoveOffSet=20;
        this.sprite = new cc.Sprite(res.Sprite_Image);
        this.sprite.attr({
            x: size.width / 2,
            y: size.height / 2
        });
        this.addChild(this.sprite, 0);
        this.sprite.tag='TouchTarget';
        //Creating Event Listener Object
        var listener = cc.EventListener.create({
          event: cc.EventListener.KEYBOARD,
              swallowTouches: true,
          onKeyPressed: function (keyCode,event) {
             var target = event.getCurrentTarget();
             var position=target.getPosition();
             switch(keyCode)
             {
                case KeyCode.LEFT:
                position.x-=MoveOffSet;
                break;
                case KeyCode.RIGHT:
                position.x+=MoveOffSet;
                break;
                case KeyCode.UP:
                position.y+=MoveOffSet;
                break;
                case KeyCode.DOWN:
                position.y-=MoveOffSet;
                break;
             }
             target.setPosition(position);
        },
        onKeyReleased: function (event) {
           cc.log('Key released');
        }
     });
     //Added Event Listener To Sprite
     cc.eventManager.addListener(listener, this.sprite);
     return true;
    }
});
```

### 4.5 加速度计事件

加速度计事件通过事件名称 `cc.EventListener.ACCELERATION` 来引用。加速度计输入需要通过输入管理器启用，如下所示：

```javascript
cc.inputManager.setAccelerometerEnabled(true);
```

之后，您可以创建一个监听器，并通过 `eventManager` 将其附加到精灵上：

```javascript
cc.eventManager.addListener({
     event: cc.EventListener.ACCELERATION,
     callback: function(acc, event){
         // Processing logic here
     }
}, sprite);
```

### 4.6 自定义事件

除了系统定义的事件之外，我们还可以创建自己的事件。请参见以下代码片段：

```javascript
var listener = cc.EventListener.create({
      event: cc.EventListener.CUSTOM,
      eventName: "my_custom_event",
      callback: function(event){
          cc.log("Custom event 1 received, " + event.getUserData() + "times");
      }
});
cc.eventManager.addListener(listener, 1);
```

这个自定义事件只有一个回调函数，该函数会在事件被触发后被调用：

```javascript
var event = new cc.EventCustom("my_custom_event");
event.setUserData(counter.toString());
cc.eventManager.dispatchEvent(event);
```

事件管理器的 `dispatchEvent` 函数用于触发自定义事件。Cocos2d 引擎内部也使用此方法来触发用户输入事件。