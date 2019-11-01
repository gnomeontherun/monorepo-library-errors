# Angular Monorepo Library Errors with Ivy

In the latest v9 of Angular, a monorepo style project built with the CLI doesn't properly build with Ivy when using a library in an application.

The following steps will create a fresh CLI project.

```bash
npm i -g @angular/cli@next
ng new monorepo-v9rc0 --createApplication=false
cd monorepo-v9rc0
ng update @angular/core@next @angular/cli@next --force
ng generate application dev --defaults
ng generate library shared
```

Then import the library into the dev `AppModule`.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { SharedModule } from '../../../shared/src/lib/shared.module';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    SharedModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Finally, run `ng build --prod dev` to see the monorepo bug again.

```bash
➜  monorepo git:(master) ✗ ng build --prod

ERROR in Unable to write a reference to SharedComponent in /monorepo/projects/shared/src/lib/shared.component.ts from /monorepo/projects/shared/src/lib/shared.module.ts
```
