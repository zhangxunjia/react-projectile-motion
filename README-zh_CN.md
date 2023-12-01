[English](./README.md) | 简体中文

## ✨ 效果

<center><img src="https://raw.githubusercontent.com/zhangxunjia/pictures/main/react-projectile-motion/projectile.gif" alt="projectile" style="width: 100%;" /></center>

## 👀️ 介绍

- 本组件为抛物运动组件理论上运动轨迹可从页面的任意一点到另外任意一点
- projectile 从传入的startingDom中心位置开始触发 至 endingDom中心位置结束

<center><img src="https://raw.githubusercontent.com/zhangxunjia/pictures/main/react-projectile-motion/projectile.png" alt="projectile" style="width: 100%;" /></center>

## 🔨 安装

```bash
npm install react-projectile-motion
```

## ❗依赖
pubsub-js  v1.x及以上
react v16.8.0 及以上
react-dom v16.8.0 及以上

## 👻demo
<a href="https://zhangxunjia.github.io/react-projectile-motion-demo/" >https://zhangxunjia.github.io/react-projectile-motion-demo/</a>

## ✍️使用

### 本组件暴露两个高阶组件供用户使用分别为 **withProjectileMotionStarter** 和 **withProjectileMotion**

```jsx
import { withProjectileMotionStarter, withProjectileMotion } from 'react-projectile-motion';
```

<br />

- ### 高阶组件 withProjectileMotionStarter:

#### 子组件获得函数 triggerProjectileMotion(subscription, startingDom) 用于触发抛物运动

triggerProjectileMotion(subscription, startingDom)函数详解：

| 参数         | 说明                                                                      | 类型        | 默认值 |
| ------------ | ------------------------------------------------------------------------- | ----------- | ------ |
| subscription | （必传）pubsub订阅名称 这里传参ProjectileMotion中的props.subscription一致 | string      |        |
| startingDom  | （必传）projectile起始位置的dom                                               | object(dom) |        |

- ### 高阶组件 withProjectileMotion:

#### 子组件获得函数 setProjectileMotionPorps(props) 注意props是对象{}  用于设置抛物运动的属性

setProjectileMotionPorps(props) 函数详解:

| 参数                                    | 说明                                                                                                                                                                   | 类型        | 默认值     |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | ---------- |
| props.subscription                      | （必传）pubsub订阅名称 这里传参ProjectileMotionStarter中的subscription一致                                                                                             | string      |            |
| props.endingDom                         | （必传）projectile结束位置的dom                                                                                                                                            | object(dom) |            |
| props.muiltipleProjectile               | 是否允许出现多个projectile                                                                                                                                                 | boolean     | true       |
| props.projectile                        | projectile（如果要添加className需把样式写在全局）                                                                                                                          | ReactNode   |            |
| props.duration                          | 抛掷动画持续的时间（单位:秒）                                                                                                                                                     | number      | 1         |
| props.zIndex                            | 设置projectile的zIndex                                                                                                                                                     | number      | 2147483647 |
| props.needEndingDomAnimation            | endingDom是否需要被projectile击中后动画                                                                                                                                    | boolean     | true       |
| props.projectileMovmentEnd              | 抛物运动动画结束回调                                                                                                                                                   | function    |            |
| props.endingDomAnimationEnd             | endingDom被击中产生的动画的结束回调                                                                                                                                                  | function    |            |
| props.endingDomAnimationDuration        | endingDom被projectile击中后动画持续时间 （单位:秒）                                                                                                                                   | number      |            |
| props.endingDomAnimationName            | endingDom被projectile击中后的animation的名称（animation需定义在全局）                                                                                                          | string      |            |
| props.additionalTransformValueInAnimate | 补充的动画的transform值，传入该值后会生成新的类名，该类会整合endingDomAnimationName对应的keyframe除最后一帧外的其他帧，形成新的类，然后供endingDom应用。可以设置rotate scale translate skew 等值，若设置多个请用空格隔开。（[图解详情](https://raw.githubusercontent.com/zhangxunjia/pictures/main/react-projectile-motion/additionalTransformValueInAnimate_zh.png)） | string      |            |
| props.wrapClassName                     | projectile外层容器的类名                                                                                                                                                   | string      |            |
| props.isInitialYAxisReverse             | 抛物运动初速度y轴方向是否为反方向                                                                                                                                      | boolean     | true       |
| props.projectileTransition              | 自定义projectile的transition属性若传入此属性则duration和isInitialYAxisReverse将失效                                                                                        | string      |            |

## ⌨️使用示例:

#### 抛物运动起始端:

```jsx
// 抛物运动起始端
import React, { useRef } from 'react';
// 引入withProjectileMotionStarter
import { withProjectileMotionStarter } from 'react-projectile-motion';

const StartCom = (props) => {
    const startingDom = useRef();
    return (
        <div
            ref={startingDom} 
            onClick={() => {
              {/* 触发抛物运动 */}
              props.triggerProjectileMotion(
                {/* 这个subscriptionName 要和 结束端props.setProjectileMotionPorps中的subscriptionName 对应 */}
                'subscriptionName', 
                startingDom.current
              )
            }}
        >
            +
        </div>
    );
};

// 使用withProjectileMotionStarter
export default withProjectileMotionStarter(StartCom);
```

#### 抛物运动结束端:

```jsx
// 抛物运动结束端
import React, { useRef, useEffect } from 'react';

// 引入withProjectileMotion
import { withProjectileMotion } from 'react-projectile-motion';

const EndCom = (props) => {
    const endingDom = useRef();

    useEffect(() => {
        // 注意:接收参数为对象
        props.setProjectileMotionPorps({
            // 这个subscription 要和起始端中props.triggerProjectileMotion的subscriptionName对应
            subscription: 'subscriptionName',
            endingDom: endingDom.current,
        });
    }, []);

    return (
        <img
            ref={endingDom}
            src={require('./shopCar.png')}
            alt="shopcar"
        />
    );
};

// 使用withProjectileMotion
export default withProjectileMotion(EndCom);
```
