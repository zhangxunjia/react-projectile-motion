English | [简体中文](./README-zh_CN.md)

## ✨ Effect

<center><img src="https://raw.githubusercontent.com/zhangxunjia/pictures/main/react-projectile-motion/projectile.gif" alt="projectile" style="width: 100%;" /></center>

## 👀️ Introduce

- This component is a projectile motion component. Theoretically, the motion trajectory can be from any point on the page to any other point.
- projectile starts from the center position of the incoming startingDom and ends at the center position of endingDom

<center><img src="https://raw.githubusercontent.com/zhangxunjia/pictures/main/react-projectile-motion/projectile.png" alt="projectile" style="width: 100%;" /></center>

## 🔨 Install

```bash
npm install react-projectile-motion
```

## ❗Requirements
pubsub-js  version 1.x and above
react  version 16.8.0 and above 
react-dom  version 16.8.0 and above

## 👻 Demo
<a href="https://zhangxunjia.github.io/react-projectile-motion-demo/" >https://zhangxunjia.github.io/react-projectile-motion-demo/</a>

## ✍️ Usage

### This component exposes two high-level components for users to use: **withProjectileMotionStarter** and **withProjectileMotion**

```jsx
import { withProjectileMotionStarter, withProjectileMotion } from 'react-projectile-motion';
```

<br />

- ### High-order components withProjectileMotionStarter:

#### The subcomponent obtains the function triggerProjectileMotion(subscription, startingDom) to trigger projectile motion

triggerProjectileMotion(subscription, startingDom)：

| parameter         | description                                                                      | type        | default value |
| ------------ | ------------------------------------------------------------------------- | ----------- | ------ |
| subscription | （required）This is the pubsub subscription name. The parameters passed here need to be consistent with props.subscription in withProjectileMotion | string      |        |
| startingDom  | （required）DOM of starting position of projectile motion                                              | object(dom) |        |

- ### High-order components withProjectileMotion:

#### subcomponent get function setProjectileMotionPorps(props) ,Please note that props are objects,  The properties of this object are used to set the properties of the projectile motion.

setProjectileMotionPorps(props) :

| parameter                                    | description                                                                                                                                                                   | type        | default value     |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | ---------- |
| props.subscription                      | （required）This is the pubsub subscription name. The parameters passed here need to be consistent with subscription in withProjectileMotionStarter                                                                                             | string      |            |
| props.endingDom                         | （required）DOM of ending position of projectile motion                                                                                                                                            | object(dom) |            |
| props.muiltipleProjectile               | Whether multiple projectiles are allowed                                                                                                                                                | boolean     | true       |
| props.projectile                        | projectile（If you want to add className, you need to write the style globally）                                                                                                                          | ReactNode   |            |
| props.duration                          | The duration of the projectile motion (Unit: second)                                                                                                                                                      | number      | 1         |
| props.zIndex                            | The zIndex of projectile                                                                                                                                                     | number      | 2147483647 |
| props.needEndingDomAnimation            | Does endingDom need to animate after being hit by a projectile                                                                                                                                    | boolean     | true       |
| props.projectileMovmentEnd              | Projectile motion animation end callback                                                                                                                                                   | function    |            |
| props.endingDomAnimationEnd             | endingDom animation end callback                                                                                                                                                  | function    |            |
| props.endingDomAnimationDuration        | endingDom animation duration after being hit by a projectile  (Unit: second)                                                                                                                                   | number      |            |
| props.endingDomAnimationName            | The name of the animation after endingDom is hit by a projectile (animation needs to be global)                                                                                                          | string      |            |
| props.additionalTransformValueInAnimate | Supplementary animation transform value, after passing in this value, a new class name will be generated. This class will integrate the other frames except the last frame of the keyframe corresponding to endingDomAnimationName to form a new class, which can then be used by endingDom. You can set values such as rotate scale translate skew. If you set multiple values, please separate them with spaces. （[Graphical details](https://raw.githubusercontent.com/zhangxunjia/pictures/main/react-projectile-motion/additionalTransformValueInAnimate_en.png)） | string      |            |
| props.wrapClassName                     | The class name of the outer container of the projectile                                                                                                                                                   | string      |            |
| props.isInitialYAxisReverse             | Is the initial velocity of projectile motion in the opposite direction of the y-axis                                                                                                                                     | boolean     | true       |
| props.projectileTransition              | This is the transition attribute of the projectile. If this attribute is passed in, the duration and isInitialYAxisReverse will be invalid.                                                                                        | string      |            |

## ⌨️Example:

#### Starting point of projectile motion:

```jsx
// Starting point of projectile motion
import React, { useRef } from 'react';
import { withProjectileMotionStarter } from 'react-projectile-motion';

const StartCom = (props) => {
    const startingDom = useRef();
    return (
        <div
            ref={startingDom} 
            onClick={() => {
              props.triggerProjectileMotion(
                {/* This subscriptionName should correspond to the subscriptionName in the ending point of props.setProjectileMotionPorps */}
                'subscriptionName', 
                startingDom.current
              )
            }}
        >
            +
        </div>
    );
};

// Use withProjectileMotionStarter
export default withProjectileMotionStarter(StartCom);
```

#### Ending point of projectile motion:

```jsx
// Ending point of projectile motion
import React, { useRef, useEffect } from 'react';
import { withProjectileMotion } from 'react-projectile-motion';

const EndCom = (props) => {
    const endingDom = useRef();

    useEffect(() => {
        // Note: the parameter received is an object
        props.setProjectileMotionPorps({
            // This subscription should correspond to the subscriptionName in the starting end of props.triggerProjectileMotion.
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

// Use withProjectileMotion
export default withProjectileMotion(EndCom);
```
