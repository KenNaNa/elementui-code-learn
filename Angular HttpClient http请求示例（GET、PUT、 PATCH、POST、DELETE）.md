[Angular HttpClient http请求示例（GET、PUT、 PATCH、POST、DELETE）](https://blog.csdn.net/wanglui1990/article/details/79171202)

# app.component.html

```html
<button (click)="getWithNoPara()"  >无参GET</button>
<button (click)="getWithpara()"  >带参GET</button>
<button (click)="getWithParaFromString()">  带参GET(FromString)</button>
<button (click)="getWithHttpHeader()"  >无参GET带httpHeader</button>
<button (click)="put()" >put替换</button>
<button (click)="patch()" >patch更新</button>
<button (click)="delete()" >delete删除</button>
<button (click)="post()" >post新增</button>

<ul *ngIf="customersObservable | async as customers else empty">
  <li *ngFor="let customer of customers">
  {{customer|json}}
  </li> 
</ul>
```

# app.component.ts

```ts
import { Component, OnInit } from '@angular/core';
import { Observable } from "rxjs/Observable";
import { HttpClient, } from "@angular/common/http";
import { HttpParams } from "@angular/common/http";
import { HttpHeaders } from '@angular/common/http';

class Customer {
  id: number;
  name: string;
  email: string;
  tel: string;
}
/*
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
*/
@Component({
  selector: 'customers',
  templateUrl: './app.component.html'
}
)

export class AppComponent implements OnInit {
  customersObservable: Observable<Customer[]>;

  title = 'app';
  constructor(private httpClient: HttpClient) {
  }
  ngOnInit() {
    /*this.customersObservable = this.httpClient
     .get<Customer[]>("http://127.0.0.1:3000/customers");*/
  }
  getWithNoPara() {
    this.customersObservable = this.httpClient
      .get<Customer[]>("http://127.0.0.1:3000/customers");
  }
  getWithpara() {
    const params = new HttpParams().set('_page', "1").set('_limit', "1");
    this.customersObservable = this.httpClient
      .get<Customer[]>("http://127.0.0.1:3000/customers", { params });
  }
  getWithParaFromString() {
    const params = new HttpParams({ fromString: '_page=1&_limit=1' });
    this.customersObservable = this.httpClient
      .get<Customer[]>("http://127.0.0.1:3000/customers", { params });
  }
  getWithHttpHeader() {
    const headers = new HttpHeaders().set("X-CustomHttpHeader", "CUSTOM_VALUE");
    this.customersObservable = this.httpClient.get<Customer[]>("http://127.0.0.1:3000/customers", { headers });

  }
  put() {
    this.httpClient.put("http://127.0.0.1:3000/customers/1",
      {
        "name": "NewCustomer001",
        "email": "newcustomer001@email.com",
        "tel": "0000252525"
      })
      .subscribe(
        data => {
          console.log("PUT Request is successful ", data);
        },
        error => {
          console.log("Rrror", error);
        }
      );
  }
  patch() {
    this.httpClient.patch("http://127.0.0.1:3000/customers/1",
      {
        "email": "newcustomer001@gmail.com"
      })
      .subscribe(
        data => {
          console.log("PUT Request is successful ", data);
        },
        error => {
          console.log("Error", error);
        }
      );
  }
  delete() {
    this.httpClient.delete("http://127.0.0.1:3000/customers/1")
      .subscribe(
        data => {
          console.log("DELETE Request is successful ", data);
        },
        error => {
          console.log("Error", error);
        }
      );
  }
  post() {

    let cs: Customer = new Customer();
    cs.id = 1;
    cs.email = "customer001@Gxmail.com";
    cs.tel = "0000001x";

    this.httpClient.post("http://127.0.0.1:3000/customers",
      cs
      /*{
          "name": "Customer004",
          "email": "customer004@email.com",
          "tel": "0000252525"
      }*/
    )
      .subscribe(
        data => {
          console.log("POST Request is successful ", data);
        },
        error => {
          console.log("Error", error);
        }
      );
  }
}
```

# app.module.ts

```ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { HttpClientModule } from '@angular/common/http';
import { AppComponent } from './app.component';


@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```



