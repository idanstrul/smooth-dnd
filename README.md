# smooth-dnd

A fast and lightweight drag&drop, sortable library for with many configuration options covering many d&d scenarios. There is no external dependencies. It uses css transitions for animations so it's hardware accelerated whenever possible.

For **React** components and usage follow <a href="https://github.com/kutlugsahin/react-smooth-dnd/">link</a>  

**Angular** and **Vue.js** wrappers are coming soon!

**Show, don't tell !**
### <a href="https://kutlugsahin.github.io/smooth-dnd-demo/">Demo page</a>

### Installation

```shell
npm i smooth-dnd
```

## Usage

#### HTML



```html
<div id="container">
  <div>Draggable 1</div>
  <div>Draggable 2</div>
  <div>Draggable 3</div>
</div>
```

```js
var containerElement = document.getElementById('container');

var container = SmoothDnD(containerElement, options);
```

## API

### SmoothDnD

Global function to convert element to a Drag and Drop container.
```js
var container = SmoothDnD(containerElement, options);
```
#### Parameters
- **containerElement** : `DOMElement` : The parent element that contains the elements to be dragged
- **options** : `object` : Set of parameters described below.
#### Returns
- **container** : `object` : handle the the created container which contains **dispose** function.
  - **dispose** : `function` : function to be called to remove detach SmoothDND form the container. It should be called when removing the **containerElement** from the DOM.


## Options

| Property | Type | Default | Description |
|-|:-:|:-:|-|
| orientation |string|`vertical` | Orientation of the container. Can be **horizontal** or **vertical**.|
|behaviour|string|`move`| Property to describe weather the dragging item will be moved or copied to target container. Can be **move** or **copy**.|
|groupName|string|`undefined`|Draggables can be moved between the containers having the same group names. If not set container will not accept drags from outside. This behaviour can be overriden by shouldAcceptDrop function. See below.
|lockAxis|string|`undefined`|Locks the movement axis of the dragging. Possible values are **x**, **y** or **undefined**.
|dragHandleSelector|string|`undefined`|Css selector to test for enabling dragging. If not given item can be grabbed from anywhere in its boundaries.|
|nonDragAreaSelector|string|`undefined`|Css selector to prevent dragging. Can be useful when you have form elements or selectable text somewhere inside your draggable item. It has a precedence over **dragHandleSelector**.|
|dragBeginDelay|number| `0` (`200` for touch devices)|Time in milisecond. Delay to start dragging after item is pressed. Moving cursor before the delay more than 5px will cancel dragging.
|animationDuration|number|`250`|Animation duration in milisecond. To be consistent this animation duration will be applied to both drop and reorder animations.|
|autoScrollEnabled|boolean|`true`|First scrollable parent will scroll automatically if dragging item is close to boundaries.
|dragClass|string|`undefined`|Class to be added to the ghost item being dragged. The class will be added after it's added to the DOM so any transition in the class will be applied as intended.
|dropClass|string|`undefined`|Class to be added to the ghost item just before the drop animation begins.|
|onDragStart|function|`undefined`|*See descriptions below*|
|onDrop|function|`undefined`|*See descriptions below*|
|getChildPayload|function|`undefined`|*See descriptions below*|
|shouldAnimateDrop|function|`undefined`|*See descriptions below*|
|shouldAcceptDrop|function|`undefined`|*See descriptions below*|
|onDragEnter|function|`undefined`|*See descriptions below*|
|onDragLeave|function|`undefined`|*See descriptions below*|

---

### onDragStart

The function to be called only by the container which drag starts from.
```js
function onDragStart(index, payload) {
  ...
}
```
#### Parameters
- **index** : `number` : index of the child item
- **payload** : `object` : the payload object that is returned by getChildPayload function. It will be undefined in case getChildPayload is not set.

### onDrop

The function to be called by any relevant container when drop is over. (After drop animation ends). Source container and any container that could accept drop is considered relevant. **dropResult** is the only parameter passed to the function which contains the following properties.
```js
function onDrop(dropResult) {
  const { removedIndex, addedIndex, payload, element } = dropResult;
  ...
}
```
#### Parameters
- **dropResult** : `object`
	- **removedIndex** : `number` : index of the removed children. Will be `null` if no item is removed. 
	- **addedIndex** : `number` : index to add droppped item. Will be `null` if no item is added. 
	- **payload** : `object` : the payload object retrieved by calling *getChildPayload* function.
	- **element** : `DOMElement` : the DOM element that is moved 

### getChildPayload

The function to be called to get the payload object to be passed **onDrop** function.
```js
function getChildPayload(index) {
  return {
    ...
  }
}
```
#### Parameters
- **index** : `number` : index of the child item
#### Returns
- **payload** : `object`

### shouldAnimateDrop

The function to be called by the target container to which the dragged item will be droppped.
Sometimes dragged item's dimensions are not suitable with the target container and dropping animation can be wierd. So it can be disabled by returning **false**. If not set drop animations are enabled.
```js
function shouldAnimateDrop(sourceContainerOptions, payload) {
  return false;
}
```
#### Parameters
- **sourceContainerOptions** : `object` : options of the source container. (parent container of the dragged item)
- **payload** : `object` : the payload object retrieved by calling *getChildPayload* function.
#### Returns
- **boolean** : **true / false**

### shouldAcceptDrop

The function to be called by all containers before drag starts to determine the containers to which the drop is possible. Setting this function will override the **groupName** property and only the return value of this function will be taken into account.

```js
function shouldAcceptDrop(sourceContainerOptions, payload) {
  return true;
}
```
#### Parameters
- **sourceContainerOptions** : `object` : options of the source container. (parent container of the dragged item)
- **payload** : `object` : the payload object retrieved by calling *getChildPayload* function.
#### Returns
- **boolean** : **true / false**

### onDragEnter

The function to be called by the relevant container whenever a dragged item enters its boundaries while dragging.
```js
function onDragEnter() {
  ...
}
```

### onDragLeave

The function to be called by the relevant container whenever a dragged item leaves its boundaries while dragging.
```js
function onDragLeave() {
  ...
}
```