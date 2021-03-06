### 给予 angular6.0 实现的一个【组件懒加载功能】

> 我们常常会遇到这样一个问题，当我们使用一个第三方控件库的时候，我们只用到了其中 1 个或某几个组件，会连带一大堆无用的东西，造成体积臃肿不堪。又或者首页用到的组件较多，首页加载速度缓慢，这个时候，我们或许需要加载用户可视范围内用到的组件，随着用户的浏览下拉，我们再去加载这些组件，渐进式加载,渐进式体验，这个时候你或许就用到了本工具所实现的功能。或者一个页面的某些不重要区域，比如第三方广告又或者不重要的元素，可以采用懒加载懒渲染，降低用户首屏等待时间。一切都在用户不知不觉中进行。大大增加用户体验，特别是中大型项目，优化必备！
> [项目地址github](https://github.com/iwe7/iwe7-lazy-load)

## 安装

```
yarn add iwe7-lazy-load
```

## 使用

```ts
import { Iwe7LazyLoadModule, LazyComponentsInterface } from 'iwe7-lazy-load';
// 用到的懒加载组件
let lazyComponentsModule: LazyComponentsInterface[] = [
  {
    // 组件的selector
    path: 'lazy-test',
    // 组件的相对地址
    loadChildren: './lazy-test/lazy-test.module#LazyTestModule'
  }
];
@NgModule({
  imports: [Iwe7LazyLoadModule.forRoot(lazyComponentsModule)],
  // 注意加上这些
  schemas: [CUSTOM_ELEMENTS_SCHEMA, NO_ERRORS_SCHEMA]
})
export class AppModule {}
```

```html
<div #ele>
  <lazy-test></lazy-test>
</div>
```

```ts
import { LazyLoaderService } from 'iwe7-lazy-load';


@ViewChild('ele') ele: ElementRef;
constructor(
  public lazyLoader: LazyLoaderService,
  public view: ViewContainerRef
) {}

ngOnInit() {
  // 开始渲染懒组件
  this.lazyLoader.init(this.ele.nativeElement, this.view);
}
```

### 定义懒加载组件 demo

```ts
import { LazyComponentModuleBase } from 'iwe7-lazy-load';
@Component({
  selector: 'lazy-test',
  template: ` i am a lazy`
})
export class LazyTestComponent {}

@NgModule({
  imports: [
    RouterModule.forChild([{
      path: '',
      component: LazyTestComponent
    }])
  ],
  declarations: [LazyTestComponent]
})
export class LazyTestModule extends LazyComponentModuleBase {
  getComponentByName(key: string): Type<any> {
    return LazyTestComponent;
  }
}
```

### 小结
> 希望对大家有所帮助
