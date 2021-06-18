[Angular7 国际化多语言i18n配置](https://www.jianshu.com/p/2737ee7f5f7a)

```js
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { APP_INITIALIZER, Injector, NgModule } from '@angular/core';

import { LayoutModule } from './layout/layout.module';

import { AppRoutingModule } from './app-routing.module';

import { AppComponent } from './app.component';
import { HttpClient, HttpClientModule } from '@angular/common/http';
import { AppService } from './app.service';
import { HttpService } from './core/net/http.service';
import { httpInterceptorProviders } from './core/net';
import { AuthService } from './core/auth.service';
import { StartupService } from './core/startup/startup.service';
import { TranslateHttpLoader } from '@ngx-translate/http-loader';
import { environment } from 'src/environments/environment';
import { TranslateLoader, TranslateModule, TranslateService } from '@ngx-translate/core';
import { TiAlertModule, TiButtonModule, TiMessageModule } from '@cloud/tiny3';

import { SharedModule } from './share/shared-module';



/**
 * 启动任务
 */
export function StartupServiceFactory(startupService: StartupService) {
  return () => startupService.load();
}

const APPINIT_PROVIDES = [
  StartupService,
  {
    provide: APP_INITIALIZER,
    useFactory: StartupServiceFactory,
    deps: [StartupService],
    multi: true
  }
];

/**
 * i18n加载方法
 * @param http http请求对象
 * @constructor 返回loader
 */
export function I18nLoaderFactory(http: HttpClient) {
  const i18nPath = environment.production ? '/static/occ/assets/i18n/' : '/assets/i18n/';
  return new TranslateHttpLoader(http, i18nPath);
}

// 配置文件
const langFileDef = {
  'en-us': ['common', 'occas', 'occdlv', 'occdm'],
  'zh-cn': ['common', 'occas', 'occdlv', 'occdm'],
};

/**
 * 初始化国际化相关内容,加载国际化资源文件完成，设定默认语言
 * @param translate 翻译服务
 * @param injector 注入对象，检索注入内容
 */
export function appInitializerFactory(translate: TranslateService, injector: Injector) {
  /**
   * 递归调用加载国际化文件的方法， 加载的文件列表查看langFileDef定义
   * @param langNames 语言类别名称列表
   * @param index 语言类别的遍历下标
   * @param subIndex 语言下各业务配置文件的名称
   */
  function loadI18nFile(langNames: Array<string>, index: number = 0, subIndex: number = 0) {
    if (index + 1 > langNames.length) {
      // 遍历完成，退出
      return;
    }
    const langName = langNames[index];
    // 加载语言文件，并合并到当前语言类别下
    return translate.getTranslation(`${langName}/${langFileDef[langName][subIndex]}`).toPromise().then(res => {
      translate.setTranslation(langName, res, true);
    }).then(() => {
      let isSwitchLang = false;
      if (subIndex + 1 >= langFileDef[langName].length) {
        isSwitchLang = true;
      }
      // 递归调用，继续加载
      return loadI18nFile(langNames, isSwitchLang ? index + 1 : index, isSwitchLang ? 0 : subIndex + 1);
    });
  }

  return () => {
    loadI18nFile(Object.keys(langFileDef)).finally(() => {
      translate.setDefaultLang('en-us');
      return translate.use('en-us').toPromise();
    });
  };
}

/**
 * 国际化文件加载Provider
 */
const LANG_PROVIDES = [
  {
    provide: APP_INITIALIZER,
    useFactory: appInitializerFactory,
    deps: [TranslateService, Injector],
    multi: true,
  },
];

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    // import HttpClientModule after BrowserModule.
    HttpClientModule,
    LayoutModule,
    AppRoutingModule,  // 主路由模块
    TranslateModule.forRoot({
      loader: {
        provide: TranslateLoader,
        useFactory: I18nLoaderFactory,
        deps: [HttpClient],
      },
    }),
    TiButtonModule,
    TiAlertModule,
    TiMessageModule,
    SharedModule
  ],
  providers: [
    ...LANG_PROVIDES,
    AuthService,
    AppService,
    HttpService,
    httpInterceptorProviders,
    APPINIT_PROVIDES
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }

```
