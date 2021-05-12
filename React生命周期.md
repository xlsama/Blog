# React生命周期

[生命周期图谱](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

## `componentDidUpdate` 和 `static getDerivedStateFromProps` 的区别

`componentDidUpdate`是在`render`之后触发，`getDerivedStateFromProps`是在`render`之前触发

**比如要接收`props`来更新state**，那就要在`render`之前更新，所以要在`getDerivedStateFromProps`中去修改state，如果在`componentDidUpdate`中去接收`props`更新state，那就会触发两次`render`，第一次是旧数据，第二次才是新数据。