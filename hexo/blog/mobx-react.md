---
title: 在 React 中使用 mobx 进行状态管理
date: 2019-10-14
tags: React
categories: React
---

observable 
inject 
computed 
action 

autorun
RunInAction 
Reaction 

尽量使用箭头函数

调试

toJS 需要引入 => 不如直接 JSON.stringify()方便

不用使用数组索引或者任何将来可能会改变的值作为 key 。如果需要的话为你的对象生成 ids。

[参考技巧](https://cn.mobx.js.org/best/react-performance.html)

<!-- 中文排序 -->

```js 
var array = [{name:'武汉'}, {name: '北京'}, {name:'上海'}, {name:'天津'}];
var resultArray = array.sort(
    function compareFunction(param1, param2) {
        return param1.name.localeCompare(param2.name,"zh");
    }
);
console.log(resultArray);
```

<!-- computed 计算属性 -->
根据现有的状态或其它计算值衍生出的值，响应式的产生一个可以被其它 observer 使用的值
```js
import {observable, computed} from "mobx";

class OrderLine {
    @observable price = 0;
    @observable amount = 1;

    constructor(price) {
        this.price = price;
    }
  // computed 不支持箭头函数的写法
    @computed get total() {
        return this.price * this.amount;
    }
}
```

<!-- autorun -->

想不产生一个新值，而达到一个效果，请使用 autorun。 举例来说，效果是像打印日志、发起网络请求等这样命令式的副作用。

autorun 中的值必须要手动清理才行


<!-- runInAction -->

```js
    // 请求底部的 table Plan 绑定数据
    @action fetchShopPlanList = (): ActionResponse => {
        const [begin, end] = this.dateRange;
        return http
            .post(ApiBaseinfo_Admin_Restaurant_restaurantID_menuCalendarByDateRange, {
                ':restaurantID': globalStore.currentShopID,
                begin,
                end,
            })
            .then(({ error, data }) => {
                if (error) {
                    return { error };
                }

                runInAction(() => {
                    const { menuCalendarList } = data;
                    // 遍历筛选绑定菜单项
                    this.boundObj = menuCalendarList.reduce((preObj, mealPlan) => {
                        const { color, name, date } = mealPlan;
                        if (color) {
                            preObj[`${date}_${name}`] = color;
                        }

                        return preObj;
                    }, {});
                    // 在一个请求的 action中直接去修改 state,可能会存在mobx 检测不到的情况，需要包裹 runInAction
                    this.shopCalendarList = data.menuCalendarList;
                });

                return { data };
            });
    };
```

