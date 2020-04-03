1.点击改变选中

```
html: :class="{active:selectedIndex===index}"
        @click="changeIndex(index)"
data: selectedIndex: 0
methods:  changeIndex (index) {
      this.selectedIndex = index
    },

```

