# 前端编码Vue风格指南

## 必须准守的

* 组件名应该始终为多个单词组成的，这样可以避免和 `HTML` 元素冲突（根组件 `App` 除外）。
* 组件的 `data` 属性必须是一个函数，而它的值必须是返回一个对象。

```js
export default {
  data () {
    return {
      foo: 'bar'
    }
  }
}
```

* `Prop` 定义应该尽量详细
* 总是为 `v-for` 设置键值 `key`
* 顶级 `App` 组件和布局组件中的样式可以是全局的，单文件组件应该设置 `scoped`
* 在插件、混入等扩展中始终为自定义的私有属性使用 `$_ `前缀。并附带一个命名空间以回避和其它作者的冲突 (比如 `$yourPluginName`)。

## B：强烈推荐

* 当构建系统能够使用不同的文件连接使用，最好将组件独立成单独的文件，这样能够在需要的时候快速的找到
* 单文件组件的文件名应该要么始终是单词大写开头 (`PascalCase`)，要么始终是横线连接 (`kebab-case`)。
* 应用特定样式和约定的基础组件 (也就是展示类的、无逻辑的或无状态的组件) 应该全部以一个特定的前缀开头，比如 `Base、App 或 V`。
* 只应该拥有单个活跃实例的组件应该以 `The` 前缀命名，以示其唯一性。比如头部文件和左侧栏

```js
components/
|- TheHeading.vue
|- TheSidebar.vue
```

* 和父组件紧密耦合的子组件应该以父组件名作为前缀命名。
* 组件名应该以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。
* 在单文件组件、字符串模板和 `JSX` 中没有内容的组件应该是自闭合的——但在 `DOM` 模板里永远不要这样做。
* 在单文件组件和字符串模板中组件名应该总是 `PascalCase` 的。但是在 `DOM` 模板中总是 `kebab-case` 的。
* JS/JSX 中的组件名应该始终是 `PascalCase` 的，尽管在较为简单的应用中只使用`Vue.component`
* 进行全局组件注册时，可以使用 `kebab-case` 字符串。
* 组件名应该尽量使用完整单词而不是缩写。
* 在声明 `prop` 的时候，其命名应该始终使用 `camelCase`，而在模板和 `JSX` 中应该始终使用 `kebab-case`。
* 多个特性的元素应该分多行撰写，每个特性一行。

```html
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
```

* 组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。
* 应该把复杂计算属性分割为尽可能多的更简单的属性。
* 非空 `HTML` 特性值应该始终带引号
* 指令缩写 (用 `:` 表示 `v-bind:` 和用 `@` 表示 `v-on:`) 应该要么都用要么都不用。

## C：一般推荐

* 组件/实例的选项的顺序

  1. 副作用 (触发组件外的影响)
    `el`

  2. 全局感知 (要求组件以外的知识)
     * `name`
     * `parent`

  3. 组件类型 (更改组件的类型)
     * functional

  4. 模板修改器 (改变模板的编译方式)
     * delimiters
     * comments
  
  5. 模板依赖 (模板内使用的资源)
     * components
     * directives
     * filters
  
  6. 组合 (向选项里合并属性)
     * extends
     * mixins
  
  7. 接口 (组件的接口)
     * inheritAttrs
     * model
     * props/propsData
  
  8. 本地状态 (本地的响应式属性)
     * data
     * computed

  9. 事件 (通过响应式事件触发的回调)
     * watch
     * 生命周期钩子 (按照它们被调用的顺序)
         * beforeCreate
         * created
         * beforeMount
         * mounted
         * beforeUpdate
         * updated
         * activated
         * deactivated
         * beforeDestroy
         * destroyed

  10. 非响应式的属性 (不依赖响应系统的实例属性)
      * methods

  11. 渲染 (组件输出的声明式描述)
      * template/render
      * renderError

* 元素特性的顺序

  1. 定义 (提供组件的选项)
     * is
  2. 列表渲染 (创建多个变化的相同元素)
     * v-for

  3. 条件渲染 (元素是否渲染/显示)
     * v-if
     * v-else-if
     * v-else
     * v-show
     * v-cloak

  4. 渲染方式 (改变元素的渲染方式)
     * v-pre
     * v-once

  5. 全局感知 (需要超越组件的知识)
     * id

  6. 唯一的特性 (需要唯一值的特性)
     * ref
     * key
     * slot

  7. 双向绑定 (把绑定和事件结合起来)
     * v-model

  8. 其它特性 (所有普通的绑定或未绑定的特性)

  9. 事件 (组件事件监听器)
     * v-on
  10. 内容 (覆写元素的内容)
      * v-html
      * v-text

* 在组件/实例选项中加入空行，使得阅读更加的容易
* 单文件组件应该总是让 `template`、`script`和`style`标签的顺序保持一致。且`<style>`要放在最后，因为另外两个标签至少要有一个。

## D：谨慎使用

* 没有在 `v-if` / `v-if-else` / `v-else` 中使用 `key`
* 在组件中的 `scoped` 中使用元素选择器
* 父子组件通信
* 全局状态管理
