# v-component-tooltips

A vue tooltips component

## Getting Started

1.Install with NPM

```npm i v-component-tooltips```

2.Import into your vue component

```import Tooltips from 'v-component-tooltips'```

3.Wrap any element with the Tooltips component

Example: Different usage of content
```
<Tooltips content="Inline Content" >
    <div class="wrap-any-element-you-want">
        Main Area
    </div>
</Tooltips>
```
```
<Tooltips>
    <div slot="content">Slot Content</div>
        Main Area With No Wrapper
</Tooltips>
```


## Properties

|  Name   | Type  | Description  | Default  | Value  |
|  :----:  | :----:  | :----  | :----:  | :----:  |
| placement  | String | The position of the Tooltips tag show, it could be 12 different directions | bottom | 'bottom', 'top', 'right', 'left', 'top-right', 'top-left', 'right-top', 'right-bottom', 'bottom-left', 'bottom-right', 'left-top', 'left-bottom' |
| no-arrow  | Boolean | Control the triangle of the tooltips tag show or not  | true | true, false |
| content  | slot/string | You can transfer the content like an property "content", or you can use the slot with name="content" to adding things to the tag | - | - |
| offset  | Array | It only accept an array with 2 elements, offset[1] control the offset to origin top of tag box, offset[2] control the offset to origin left of tag box | [0, 0] | - |
| show-delay  | Number | The seconds before showing the tag | 0 | - |
| hide-delay  | Number | The seconds before hiding the tag | 0 | - |
| disabled  | Boolean | For controlling the tag show or not | false |true, false |
| text-color  | String | The color of main text after hovering | "rgba(255, 255, 255, 1)" | rgba, rgb, binary color |
| box-color  | String | The color of main box border after hovering | "rgba(196, 196, 196, 1)" | rgba, rgb, binary color |
| duration | Number | The duration of completing the transition function | 0.2 | - |
| transition-function  | String | The time function of transition  | "ease-in" | - |


##Events
|  Name   | Type  | Description  | Default  | Value  |
|  :----:  | :----:  | :----  | :----:  | :----:  |
| onPopperOpen  | Function | Triggered when the mouse is hover into the main area | - | - |
| onPopperClose  | Function | Triggered when the mouse leave the main area  | - | - |


## How to use(simple-examples)

>offset
```
<Tooltips placement="left" content="Left Offset Content" :offset="[-122,-111]">
      <div class="tooltips-content">
        offset in left tag
      </div>
</Tooltips>
```

> disabled
```
<Tooltips placement="top" content="有Disabled的content" disabled>
    <div class="tooltips-content">
        content with disabled
    </div>
</Tooltips>
```

> duration && transition-function
```
<Tooltips placement="left" content="abc" :duration="0.2" transition-function="ease-in">
    <div class="tooltips-content">
          transition function
      </div>
</Tooltips>
```

> show-delay && hide-delay
```
<Tooltips placement="top" content="abc" :show-delay="1" :hide-delay="2">
    <div class="tooltips-content">
        With both show delay and hide delay
    </div>
</Tooltips>
```

> no-arrow
```
<Tooltips placement="top" content="abc" no-arrow>
    <div class="tooltips-content">
        没有箭头
    </div>
</Tooltips>
```
