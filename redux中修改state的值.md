#  redux中修改state的值

场景：

现在有5个表格，每个表格中选中的id都要存放同一个数组里（这里就叫`selectedTargetList`）

单选行的时候把选中行的id存到数组里，按住`ctrl`时为多选，选中行以后点击可以取消选中

因为5个表格都是不同的组件，要共用一个list，所以要把list放到redux的`store`里。

**表格的`onClick`事件代码：**

```javascript
 // 点击行
onClick: e => {
    // 修改当前选中的目标列表  e.ctrlKey可以判断当前是否按下ctrl键
    this.props.setSelectedTargetList('sip' + record.targetId, e.ctrlKey); 
}
```

**reducer中的代码：**

```javascript
// 初始化state数据
const initialState = {
  selectedTargetList: [], // 当前选中的目标列表
};

export default (state = initialState, action) => {
  switch (action.type) {
	 case SET_SELECTED_TARGET_LIST:
      let targetId = action.id;
      /* 
        通过ctrlKey判断是否按下Ctrl键，true为多选，false为单选
        单选：返回一个数组，数组里只有一个当前选中的目标ID
        多选：判断数组中是否含有当前选择的ID，如果有就把这个id过滤掉，没有则添加
      */
      return {
        ...state,
        selectedTargetList: action.ctrlKey
          ? state.selectedTargetList.includes(targetId)
            ? state.selectedTargetList.filter(id => id !== targetId)
            : [...state.selectedTargetList, targetId]
          : [targetId],
      };
          
    default:
      return state;
  }
};
```

下面这么写是错误的：

```javascript
      return {
        ...state,
        selectedTargetList: action.ctrlKey
          ? state.selectedTargetList.includes(targetId)
            ? state.selectedTargetList.splice(
                state.selectedTargetList.findIndex(id => id === targetId),
                1,
              )
            : [...state.selectedTargetList, targetId]
          : [targetId],
      };
```

```js
console.log([1,2,3].splice(1,1)) //[2]
```

数组的`splice`方法返回的是截取的值，而且还需要传入删除元素的索引值，并且只能单个删除。

所以最好使用数组的`filter`方法来过滤（删除）数组中的某一项。

### 如何修改以redux状态存储的对象数组中的某个对象

```js
state = {
	records : [
  	 	{id:"1", name:"foo", city:"A"},
    	{id:"2", name:"bar", city:"B"},
    	{id:"3", name:"abc", city:"C"},
    ]
}
```

```js
case 'SOMETHING': 
const recordsCopy = {...state.records};
//change recordsCopy ...
const recordToChange = recordsCopy.find(x => x.id === '3')
if(recordToChange) {
   recordToChange.name = 'new name';
}
return {...state, records: [...recordsCopy]}
```