[Angular7 国际化多语言i18n配置](https://www.jianshu.com/p/2737ee7f5f7a)

# app.module.ts

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

# app.service.ts

```js
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable, Subject } from 'rxjs';
import { environment } from 'src/environments/environment';
import MessageType from './enum/message-type';

/** Mock client-side authentication/authorization service */
@Injectable({
    providedIn: 'root'
})

export class AppService {

    public userInfo;

    public isMaskLoading = false; // 全局loading框
    public isMutiMaskLoading = false; // 多请求控制loading
    /**
     * 在外部系统跳转的情况下
     * 作用：1.用来保存原来外部系统的url路径
     *      2.如果不为空，header组件则显示返回外部系统的按钮，点击时跳转到该路径
     */
    public externalJumpPath = '';

    private _i18n: any = {};
    public set i18n(val: any) {
        this._i18n = val || {};
    }
    private _tiMessage: any;
    public set tiMessage(val: any) {
        if (typeof val === 'object') {
            this._tiMessage = val;
        }
    }

    // Observable string sources
    private broadcast = new Subject<any>();
    private dispatch = new Subject<any>();
    // 父组件触发，子组件接收
    public broadcastObs: Observable<any> = this.broadcast.asObservable();
    // 子组件触发，父组件接收
    public dispatchObs: Observable<any> = this.dispatch.asObservable();


    // 当前环境配置信息
    public environmentConfig: any;
    // 当前语言环境
    public isZH_CN = false;

    // Service message commands
    public doBroadcast(msgSource: string, msgType: string, msgObj: any) {
        this.broadcast.next({
            source: msgSource,
            type: msgType,
            message: msgObj
        });
    }

    constructor(private http: HttpClient) { }


    alert(event: any) {
        this.dispatch.next(event);
    }

    sendToMessageBox(event: any) {
        this.doBroadcast('_message_box_', MessageType.message_type_error, event);
    }

    getUserInfo() {
        return this.userInfo;
    }

    setUserInfo(userInfo) {

        userInfo.roles = Array.isArray(userInfo.roles) ? userInfo.roles : [];
        this.userInfo = userInfo
    }

    /**
        * 判断是否有访问系统权限的方法
        * 如果角色列表roles中包括'approv_adm'或者'approv_act'，那么才是有权限访问的
        */
    hasVisitPermisson() {
        return this.userInfo?.roles?.includes('approv_adm') || this.userInfo?.roles?.includes('approv_act');
    }
    /**
     * html转义，防止xss
     * @param html html模板字符串
     */
    encodeHtml(html: string) {
        if (!html || html.length === 0) {
            return html;
        }
        const codeMap = [
            {
                regExp: /&/g,
                chart: '&#38;'
            },
            {
                regExp: /</g,
                chart: '&#60;'
            },
            {
                regExp: />/g,
                chart: '&#62;'
            },
            {
                regExp: / /g,
                chart: '&#160;'
            },
            {
                regExp: /\'/g,
                chart: '&#39;'
            },
            {
                regExp: /\"/g,
                chart: '&#34;'
            },
            {
                regExp: /\//g,
                chart: '&#x2F;'
            }
        ];
        return codeMap.reduce((currentHtml, codeItem) => {
            currentHtml = currentHtml.replace(codeItem.regExp, codeItem.chart);
            return currentHtml;
        }, html);
    }

    /**
     * 获取cookie中name对应的值
     * @param name 键
     */
    getCookie(name: string): string {
        const cookie = document.cookie;
        const arrCookie = cookie.split('; ');
        for (const c of arrCookie) {
            const arr = c.split('=');
            if (arr[0] === name) {
                return arr[1];
            }
        }
        return '';
    }

    /**
     * 设置cookie中key对应的值
     * @param cname cookie中的key
     * @param cvalue key对应的值
     */
    setCookie(cname, cvalue) {
        let cookieStr = cname + '=' + encodeURIComponent(cvalue);
        const expiresTime = 2592e6;
        let date;
        date = new Date();
        date.setTime(date.getTime() + expiresTime);
        const host = window.location.hostname;
        const RegUrl = /^((\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.){3}(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$/;
        const domain = RegUrl.test(host) ? host : host.substr(host.indexOf('.'));
        cookieStr += ';expires=' + date.toGMTString() + ';path=/;domain=' + domain;
        document.cookie = cookieStr + ';secure';
    }
    /**
     * 获取url的参数
     * @param name 参数名
     * @param urlStr 要匹配的地址，默认不传
     */
    getQueryString(name, urlStr?: string) {
        const reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)');
        if (urlStr && urlStr.indexOf('?') > -1) {
            urlStr = urlStr.substr(urlStr.lastIndexOf('?'));
        }
        const r = (urlStr || window.location.search).substr(1).match(reg);
        if (r != null) {
            return unescape(r[2]);
        }
        return null;
    }

    getExceptionItemTip(label: string, value?: string) {
        // value存在则展示
        const tip = value ? '<div><span>' + label + ': ' + '</span><span>' + this.encodeHtml(value) + '</span></div>' : '';
        return tip;
    }
    // 异常展示框
    showMessagewithException(exceptions: any) {
        const i18n = this._i18n;
        if (exceptions.status >= 400 && exceptions.status < 500 && exceptions.status !== 404) {
            const exception = exceptions.error;
            if (toString.call(exception) === '[object Object]') {
                this._tiMessage.open({
                    id: `_work_order_ti_message_error_${new Date().getTime()}`,
                    type: 'error',
                    title: i18n.error_log_button,
                    okButton: {
                        text: i18n.confirm_button,
                        show: true
                    },
                    cancelButton: {
                        show: false
                    },
                    content: this.getExceptionItemTip(i18n.error_code_button, exception.exceptionId)
                        + this.getExceptionItemTip(i18n.desc_label, exception.descArgs && exception.descArgs[0])
                        + this.getExceptionItemTip(i18n.reason_button, exception.reasonArgs && exception.reasonArgs[0])
                        + this.getExceptionItemTip(i18n.error_detail_button, exception.detailArgs && exception.detailArgs[0])
                        + this.getExceptionItemTip(i18n.error_advice_button, exception.adviceArgs && exception.adviceArgs[0]),
                });
            } else {
                this.alert({
                    label: i18n.error_503_label,
                    dismissOnTimeout: 3000,
                    alertType: 'error'
                });
            }
        } else {
            let tmpLabel = '';
            if (exceptions.status >= 500 || exceptions.status === 404) {
                tmpLabel = i18n.error_503_label;
            } else if (exceptions.status === 0) {
                tmpLabel = i18n.error_internet_label;
            } else if (exceptions.status === 200) {
                tmpLabel = i18n.error_inner_label;
            }
            if (tmpLabel) {
                this.alert({
                    label: tmpLabel,
                    dismissOnTimeout: 3000,
                    alertType: 'error'
                });
            }
        }
    }

    /**
     * @param textTemplate 词条模板
     * @param replaceObject 需要被替换成的真实数据
     */
    i18nReplace(textTemplate: string, replaceObject: object) {
        const subRegRex = /\{\s*([^\|\}]+?)\s*(?:\|([^\}]*))?\s*\}/g;
        return ((textTemplate.replace) ? textTemplate.replace(subRegRex, function (match, key) {
            return (replaceObject[key] !== undefined) ? replaceObject[key] : match;
        }) : textTemplate);
    }

    /**
     * 请求全部的列表数据
     * @param serviceObj 服务对象
     * @param funcName 服务下的属性
     * @param param 请求去参数书
     * @param dataKey data数组对应的key值
     */
    requestAllListData(serviceObj: any, funcName: string, param: any, dataKey: string): Promise<any> {
        return new Promise((resolves, rejects) => {
            serviceObj[funcName]({ ...param })
                .then(data => {
                    const tmpArr = [Promise.resolve(data)];
                    // 第一页数据已经请求了，所以从第二页开始全部一次请求
                    let index = 1;
                    while (index < Math.ceil(data.total / param.limit)) {
                        let tmpParam = { ...param };
                        tmpParam.start = index * param.limit;
                        tmpArr.push(serviceObj[funcName](tmpParam));
                        index++;
                    }
                    // 请求剩余的数据
                    return Promise.all(tmpArr);
                })
                .then(resp => {
                    const tmpData = [];
                    Array.isArray(resp) && resp.forEach(val => {
                        tmpData.push(...val[dataKey]);
                    });
                    resolves(tmpData);
                })
                .catch(err => {
                    rejects(err);
                });
        });
    }

    // 获取当前日期格式
    curDateFormat(isTime: boolean = true, isMoment: boolean = false) {
        let formatStr = this.isZH_CN ? 'yyyy-MM-dd' : 'MMM dd, yyyy';
        isMoment && (formatStr = formatStr.toUpperCase());
        isTime && (formatStr += ' HH:mm:ss');
        return formatStr;
    }

    // 获取当前日期格式-选择器中的日期
    curDateFormatForSelect(isTime: boolean = true) {
        let formatStr = this.isZH_CN ? 'yyyy-MM-dd' : 'MMM dd, yyyy';
        if (!isTime) {
            return formatStr;
        }
        return { date: formatStr, time: 'HH:mm:ss' };
    }

    // 图片地址适配
    static imgAddressAdaptor(src: string): string {
        if (environment.production) {
            return src.replace('/assets', '/static/occ/assets');
        } else {
            return src;
        }
    }
}
```
