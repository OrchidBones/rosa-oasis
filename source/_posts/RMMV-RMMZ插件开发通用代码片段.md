---
title: RMMZ插件开发通用代码片段
date: 2025-08-26 22:26:40
categories:
- [存档, RM开发, 插件脚本]
tags:
- RMMZ
- 代码
---
# RMMZ插件开发时开箱即用的代码片段

存档。

## 1. 新增控制字符：
   
```javascript {.line-numbers}
let __Window_Message_processEscapeCharacter = Window_Message.prototype.processEscapeCharacter;
Window_Message.prototype.processEscapeCharacter = function(code, textState) {
    switch(code) {
        case 'ECA':
            this.processEscapeCodeASpecialState(this.obtainEscapeParam(textState));
            break;
        case 'ECB':
            return 999;
            break;
        default: 
            __Window_Message_processEscapeCharacter.call(this, code, textState);
            break;
    }
};
```

<br>

## 2. 新增 Game_XXX() 类（附带 $gameXXX，存入存档）
   
```javascript {.line-numbers}
var $gameXXX = null;

let __DataManager_createGameObjects = DataManager.createGameObjects;
DataManager.createGameObjects = function() {
    __DataManager_createGameObjects.call(this);
    $gameXXX = new Game_XXX();
};

let __DataManager_makeSaveContents = DataManager.makeSaveContents;
DataManager.makeSaveContents = function() {
    const contents = __DataManager_makeSaveContents.call(this);
    contents.xxx = $gameXXX;
    return contents;
};

let __DataManager_extractSaveContents = DataManager.extractSaveContents;
DataManager.extractSaveContents = function(contents) {
    __DataManager_extractSaveContents.call(this, contents);
    $gameXXX = contents.xxx;
};

function Game_XXX() {
    this.initialize(...arguments);
};

Game_XXX.prototype.initialize = function() {
    // 
};
```

<br>

## 3. 向地图各层添加 Sprite() 实例

### 最底层（地图图块层）：

```javascript {.line-numbers}
let __Spriteset_Base_createPictures = Spriteset_Base.prototype.createPictures;
Spriteset_Base.prototype.createPictures = function() {
    this.createLowerSprite();
    __Spriteset_Base_createPictures.call(this);
};

Spriteset_Base.prototype.createLowerSprite = function() {
    this._lowerSprite = new Sprite();
    this.addChild(this._lowerSprite);
};

Spriteset_Map.prototype.createLowerSprite = function() {
    this._lowerSprite = new Sprite();
    this._lowerSprite.z = 1; // 0 - 远景层，1 - 图块层，3 - 玩家/事件层
    this._tilemap.addChild(this._lowerSprite);
};

Spriteset_Battle.prototype.createLowerSprite = function() {
    this._lowerSprite = new Sprite();
    this.updateLowerPictureContainerZ();
    this._lowerSprite.x = -this._battleField.x;
    this._lowerSprite.y = -this._battleField.y;
};

Spriteset_Battle.prototype.updateLowerPictureContainerZ = function() {
    this._battleField.addChildAt(this._lowerSprite,  0);
};
```

### 中层（图片下方、图片上方）：

```javascript {.line-numbers}
// 如果和上一段代码合用，必须在上一段之后书写这一段

let __Spriteset_Base_createPictures = Spriteset_Base.prototype.createPictures;
Spriteset_Base.prototype.createPictures = function() {
    this.createBelowPictureSprite(); // 图片层之下
    __Spriteset_Base_createPictures.call(this);
    this.createAbovePictureSprite(); // 图片层之上
};

Spriteset_Base.prototype.createBelowPictureSprite = function() {
    this._belowPictureSprite = new Sprite();
    this.addChild(this._belowPictureSprite);
};

Spriteset_Base.prototype.createAbovePictureSprite = function() {
    this._abovePictureSprite = new Sprite();
    this.addChild(this._abovePictureSprite);
};
```

### 最高层（窗口上方）：

```javascript {.line-numbers}
let __Scene_Base_createWindowLayer = Scene_Base.prototype.createWindowLayer;
Scene_Base.prototype.createWindowLayer = function() {
    __Scene_Base_createWindowLayer.call(this);
    if (this._spriteset) {
        this._spriteset.createUpperSprite(this);
    }
};

Spriteset_Base.prototype.createUpperSprite = function(parentScene) {
    this._upperSprite = new Sprite();
    parentScene.addChild(this._upperSprite);
};
```

<br>

## 4. 控制字符接收字符串参数

```javascript {.line-numbers}
Window_Base.prototype.obtainEscapeString = function(textState) {
    const arr = /^\[(.*?)\]/.exec(textState.text.slice(textState.index));
    if(arr) {
        textState.index += arr[0].length;
        return String(arr[0].slice(1, arr[0].length - 1));
    } else {
        return '';
    }
};
```