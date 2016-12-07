# ng2-admin
angular2,ng2-bootstrap,ng2-smart-table

在使用[ng2-admin](https://akveo.github.io/ng2-admin/articles/001-getting-started/)的时候遇到的问题

#### 1.  ng2-bootstarp中的modal弹出出错

>Error: ApplicationRef instance not found
解决方法：
>##### 1.  在app.module里面
><code type="typescripts">import { ComponentsHelper } from 'ng2-bootstrap';<br/>
>providers: [{<br/>
>  provide: ComponentsHelper, useClass: ComponentsHelper<br/>
>}]</code>
>##### 2.  在app.components里面
><code type="typescripts">import { ViewContainerRef } from '@angular/core';<br/>
>import { ComponentsHelper } from 'ng2-bootstrap';<br/>
>viewContainerRef: any;<br/>
>public constructor( _comhelp: ComponentsHelper, _vcr: ViewContainerRef) {<br/>
>   this.viewContainerRef = _vcr;<br/>
>   _comhelp.setRootViewContainerRef(_vcr);<br/>
>}</code>
>##### 3.  在node_modules/ng2-bootstarp/components/utils/components-helper.service里面
>修改ComponentsHelper.prototype.getRootViewContainerRef里面的
><code type="typescripts">this.root = rootComponent._hostElement.vcRef为<br/>
>this.root = rootComponent._component.viewContainerRef || rootComponent._hostElement.vcRef</code>
