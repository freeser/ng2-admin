# ng2-admin
angular2,ng2-bootstrap,ng2-smart-table

在使用[ng2-admin](https://akveo.github.io/ng2-admin/articles/001-getting-started/)的时候遇到的问题

### 1.  ng2-bootstarp中的modal弹出出错

```
error_handler.js:53 Error: ApplicationRef instance not found
    at ComponentsHelper.getRootViewContainerRef (components-helper.service.js:50)
    at ComponentsHelper.appendNextToRoot (components-helper.service.js:94)
    at ModalDirective.showBackdrop (modal.component.js:191)
    at ModalDirective.show (modal.component.js:108)
    at AppComponent.ngAfterViewInit (app.component.ts:22)
    at _View_AppComponent_Host0.detectChangesInternal (host.ngfactory.js:36)
    at _View_AppComponent_Host0.AppView.detectChanges (view.js:219)
    at _View_AppComponent_Host0.DebugAppView.detectChanges (view.js:324)
    at ViewRef_.detectChanges (view_ref.js:130)
    at application_ref.js:437
```

解决方法：

>##### 1.  在app.module里面

```
import { ComponentsHelper } from 'ng2-bootstrap';
providers: [{
  provide: ComponentsHelper, useClass: ComponentsHelper
}]
```
>##### 2.  在app.components里面

```
import { ViewContainerRef } from '@angular/core';
import { ComponentsHelper } from 'ng2-bootstrap';

export class App {
    viewContainerRef: any;
    public constructor( _comhelp: ComponentsHelper, _vcr: ViewContainerRef) {
    this.viewContainerRef = _vcr;
    _comhelp.setRootViewContainerRef(_vcr);
    }
}
```

>##### 3.  在app.components最后添加


```
ComponentsHelper.prototype.getRootViewContainerRef = function () {
    if (this.root) {
        return this.root;
    }
    let comps = this.applicationRef.components;
    if (!comps.length) {
        throw new Error(`ApplicationRef instance not found`);
    }
    try {
        let rootComponent = this.applicationRef._rootComponents[0];
        this.root = rootComponent._component.viewContainerRef || rootComponent._hostElement.vcRef;
        return this.root;
    }
    catch (e) {
        throw new Error(`ApplicationRef instance not found`);
    }
};
```
