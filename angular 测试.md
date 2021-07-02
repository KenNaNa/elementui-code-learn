- [angular-note-12-karma-config](https://godbasin.github.io/2016/07/31/angular-note-12-karma-config/)
- [configuration-file](https://karma-runner.github.io/1.0/config/configuration-file.html)
- [jasmine](https://jasmine.github.io/)
- [基于 Jasmine 和 Karma 的单元测试基础教程](https://hijiangtao.github.io/2020/07/06/Basic-Unit-Test-Tutorial/)

# Angular 单元测试

测试程序有两种主要方法：*端到端测试* 和 *单元测试*。

+ 端到端测试：使用自上而下的方法进行测试，测试时将程序视为一个黑盒，测试用例与程序交互就入真实用户一样，从一个旁观者的角度评判程序是否达标。在 Angular 中，使用 Protractor 进行端到端测试。
+ 单元测试：隔离程序的每个部件，在隔离环境中运行测试用例。在单元测试中，所写的测试需要事先提供既定的输入值与相应的逻辑单元，检测输出结果，确定它是否与我们的预期结果匹配。

本文主要介绍 Angular 单元测试。

# 测试工具

Angular 官方提供的测试工具是 Jasmine 和 Karma。

+ Jasmine：是一种用于测试 JS 代码的行为驱动框架，不依赖其他任何框架。单元测试代码中用的都是其提供的功能、函数。利用 Jasmine 可以设置代码调用后的预期结果。
+ Karma：为测试提供运行环境（比如真实的浏览器环境或者 PhantomJS 这样的空壳浏览器环境）。它使用配置文件来设置启动文件，报告，测试框架，浏览器等等。

# 配置脚本

## karma.conf.js

我们的核心是让 Karma 运行器运行起来。使用 Angular CLI 生成项目的时候会在根目录创建 karma.conf.js 文件。文件是为了告知 Karma 需要启用哪些插件、加载哪些测试脚本、需要哪些测试浏览器环境、测试报告通知方式、日志等等。具体配置内容参见 [karma.conf.js](./karma.conf.js)。

## test.ts

angular.json 中指定的测试入口文件，其中初始化了测试环境以及指定所有测试文件。


## 测试文件名及其位置

查看 src/app 文件夹。CLI 为 AppComponent 生成了一个名叫 app.component.spec.ts 的测试文件。

> 测试文件的扩展名必须是 .spec.ts，这样工具才能识别出它是一个测试文件，也叫规约（spec）文件。

app.component.ts 和 app.component.spec.ts 文件位于同一个文件夹中，而且相邻。 其根文件名部分（app.component）都是一样的。
请在你的项目中对任意类型的测试文件都坚持这两条约定。

# 运行测试

## 运行命令

现在可以启动 karma 来运行测试了。执行命令 `ng test [project|library]`。

如果要指定执行某个目录或者文件中的测试用例可以使用命令 `ng test --include='**/unit/demo.spec.ts'`

## 启用代码覆盖率报告

代码覆盖率报告会向你展示代码库中有哪些可能未使用单元测试正常测试过的代码。运行命令 `ng test --code-coverage`。

或者在 angular.json 中配置：
```
"test": {
  "options": {
    "codeCoverage": true
  }
}
```

## 命令参数

使用 `ng test --help` 查看其他相关命令参数。

+ –-code-coverage -cc 代码覆盖率报告, 默认不开启。
+ --include 指定测试目录或文件
+ --source-map -sm 生成sourcemaps 默认开启
+ –-progress 把测试的过程输出到控制台 默认开启
+ –-watch -w 运行测试一次, 并且检测变化 默认开启
+ –-ts-config typescript 配置文件
+ --main 测试入口文件


# Jasmine 介绍

Angular 单元测试是使用 Jasmine 框架来编写的，所有有必要带大家认识一下 Jasmine 的基础知识。

## 基本概念

### Describe

describe(string, function)：是 Jasmine 的全局函数，可以理解为一个测试集（Test Suite），主要功能是用来划分单元测试的。describe 可以嵌套使用。
+ 参数 string: 作为特定 Suite 的名字和标题。
+ 参数 function：实现 Suite 的代码。

```
describe("This is an exmaple suite", function() {
  it("contains spec with an expectation", function() {
    expect(true).toBe(true);
    expect(false).toBe(false);
    expect(false).not.toBe(true);
  });
});

```

### Specs

it(string, function)：可以理解为测试用例。Specs 通过调用 it 的全局函数来定义。每个 Spec 包含一个或多个 expectations 来测试需要测试代码。
+ 参数 string：描述测试用例的信息
+ 参数 function：测试用例的具体实现

Jasmine 中的每个 expectation 是一个断言，可以是 true 或者 false。当每个 Spec 中的所有 expectations 都是 true，则通过测试。有任何一个 expectation 是 false，则未通过测试。而方法的内容就是测试主体。

JavaScript 的作用域的规则适用，所以在 describe 定义的变量对 Suite 中的任何 it 代码块都是可见的。

```
describe("Test suite is a function.", function() {
  let gVar;
 
  it("Spec is a function.", function() {
    gVar = true;
    expect(gVar).toBe(true);
  });
 
  it("Another spec is a function.", function() {
    gVar = false;
    expect(gVar).toBe(false);
  });
 
});
```

### Expectations

Expectations 是由方法 expect 来定义，断言表达式。一个值代表实际值。另外的匹配的方法，代表期望值。

## Matchers

每个 Matchers 实现一个布尔值，在实际值和期望值之间比较。它负责通知 Jasmine，此 expectation 是真或者假。然后 Jasmine 会认为相应的 spec 是通过还是失败。

所有的 expec t都可以使用 not 表示否定的断言。
```
describe("The 'toBe' matcher compares with ===", function() {
  it("and has a positive case ", function() {
    expect(true).toBe(true);
  });
  it("and can have a negative case", function() {
    expect(false).not.toBe(true);
  });
});
```

### 常用的 matcher

```
  toBe()                        // 基本类型判断 等同 ===
  toNotBe()                     // 等同 !==
  toBeDefined()                 // 判断是否是 undefined 等同 !== undefined
  toBeUndefined()               // 等同 === undefined
  toBeNull()                    // 判断是否是 null  等同 === null
  toBeTruthy()                  // 判断是否能转换成 boolean 型, 判断的是否是 True  等同 !!obj
  toBeFalsy()                   // 等同 !obj
  toBeLessThan()                // 判断值类型的大小，结果若小则为True（也可以判断字符及字符串，以ascii码的大小为判断依据） 等同 <
  toBeGreaterThan()             // 等同 >
  toEqual()                     // 判断是否相等  等同 ==
  toNotEqual()                  // 等同 !=
  toContain()                   // 判断集合是否包含（可以是普通类型，和可以是对象） 等同 indexOf
  toBeCloseTo()                 // 判断数字是否相似（第二个参数为小数精度，默认为2位）
  toHaveBeenCalled()            // 用来判断一个函数是否被调用过
  toHaveBeenCalledWith()        // 检查传入参数是否被作为参数调用过
  toMatch()                     // 使用正则表达式判断  等同 new RegExp().test()
  toNotMatch()                  // 等同 !new RegExp().test()
  toThrow()                     // 判断 function 是否抛出异常
```

## Setup 和 Teardown

Jasmine 允许在执行测试集/测试用例的开始前/结束后做一些初始化/销毁的操作。

### Setup 方法：

+ beforeAll：每个 suite（即 describe）中所有 spec（即 it）运行之前运行
+ beforeEach：每个 spec（即 it）运行之前运行

### Teardown 方法：

+ afterAll：每个 suite（即 describe）中所有 spec（即 it）运行之后运行
+ afterEach：每个 spec（即 it）运行之后运行

```
describe("An example of setup and teardown)", function() {
  var gVar;
  beforeAll(function() {
      console.log('beforeAll');
  });

  beforeEach(function() {
    gVar = 3.6;
    gVar += 1;
    console.log('beforeEach');
  });
 
  afterEach(function() {
    gVar = 0;
    console.log('afterEach');
  });

  afterAll(function() {
    console.log('afterAll');
  });
 
  it("after setup, gVar has new value.", function() {
    expect(gVar).toEqual(4.6);
  });
 
  it("A spec contains 2 expectations.", function() {
    gVar = 0;
    expect(gVar).toEqual(0);
    expect(true).toEqual(true);
  });
});
```

## 跳过测试代码块

在实际项目中，需要由于发布的版本需要选择测试用例包，xdescribe和xit能很方便的将不包含在版本中的测试用例排除在外。不过xdescribe和xit略有不同：
+ xdescribe：该 describe下的所有 it 将被忽略，jasmine 将直接忽略这些it，因此不会被运行
+ xit：运行到该 it 时，挂起它不执行

```
  xdescribe("Test xdescribe", function() {
    it("Spec 1", function() {
      expect(1).toBe(1);
    });
    
    it("Spec 2", function() {
      expect(2).toBe(2);
    });
  });
  
  describe("Test xit", function() {
    it("Spec 1", function() {
      expect(1).toBe(1);
    });
    
    xit("Spec 2", function() {
      expect(2).toBe(1);
    });
    
    xit("Spec 3", function() {
      expect(3).toBe(3);
    });
  });
```
## 专注于测试代码块

测试项目时，如果只需要运行某一个测试集可以使用 fdescribe 和 fit。

```
  fdescribe("Test fdescribe", function() {
    it("Spec 1", function() {
      expect(1).toBe(1);
    });
    
    it("Spec 2", function() {
      expect(2).toBe(2);
    });
  });
  
  describe("Test fit", function() {
    it("Spec 1", function() {
      expect(1).toBe(1);
    });
    
    fit("Spec 2", function() {
      expect(2).toBe(1);
    });
    
    xit("Spec 3", function() {
      expect(3).toBe(3);
    });
  });
```


## Spy

Spy 用来追踪函数的调用历史信息（是否被调用、调用参数列表、被请求次数等）。Spy 仅存在于定义它的 describe 和 it 方法块中，并且每次在 spec 执行完之后被销毁。

```
describe("A spy", function() {
  var foo, bar = null;

  beforeEach(function() {
    foo = {
      setBar: function(value) {
        bar = value;
      }
    };

    // 在 foo 对象上添加 spy
    spyOn(foo, 'setBar'); 

    // 此时调用 foo 对象上的方法，均为模拟调用，因此不会执行实际的代码
    foo.setBar(123); // 调用 foo 的 setBar 方法
    foo.setBar(456, 'another param');
  });

  it("tracks that the spy was called", function() {
    // 判断 foo 的 setBar 是否被调用
    expect(foo.setBar).toHaveBeenCalled(); 
  });

  it("tracks all the arguments of its calls", function() {
    // 判断被调用时的参数
    expect(foo.setBar).toHaveBeenCalledWith(123);
    expect(foo.setBar).toHaveBeenCalledWith(456, 'another param');
  });

  it("stops all execution on a function", function() {
    // 由于是模拟调用，因此 bar 值并没有改变
    expect(bar).toBeNull();
  });
});
```

由上面的代码可以看到，当在一个对象上使用 spyOn() 方法后即可模拟调用对象上的函数，此时对所有函数的调用是不会执行实际代码的。

### and.callThrough()

通过在使用 spyOn 后面增加了链式调用 and.CallThrough()，这将告诉 Jasmine 我们除了要完成对函数调用的跟踪，同时也需要执行实际的代码。

```
describe("A spy, when configured to call through", function() {
    var foo, bar, fetchedBar;

    beforeEach(function() {
        foo = {
            setBar: function(value) {
                bar = value;
            },
            getBar: function() {
                return bar;
            }
        };

        // 使用 callThrough，这时所有的函数调用为真实的执行
        spyOn(foo, 'getBar').and.callThrough(); 
        // 看看测试集执行的结果
        // spyOn(foo, 'getBar'); 


        foo.setBar(123);
        fetchedBar = foo.getBar();
    });

    it("tracks that the spy was called", function() {
        expect(foo.getBar).toHaveBeenCalled();
    });

    it("should not effect other functions", function() {
        expect(bar).toEqual(123); // 由于是真实调用，因此 bar 有了真实的值
    });

    it("when called returns the requested value", function() {
        // 由于是真实调用，fetchedBar 也有了真实的值
        expect(fetchedBar).toEqual(123);
    });
});
```

### and.returnValue()

由于 Spy 是模拟函数的调用，因此我们也可以强制指定函数的返回值。

```
describe("A spy, when configured to fake a return value", function() {
  var foo, bar, fetchedBar;

  beforeEach(function() {
    foo = {
      setBar: function(value) {
        bar = value;
      },
      getBar: function() {
        return bar;
      }
    };
    // 这将指定 getBar 方法返回值为745
    spyOn(foo, "getBar").and.returnValue(745); 
    foo.setBar(123);
    fetchedBar = foo.getBar();
  });

  it("tracks that the spy was called", function() {
    expect(foo.getBar).toHaveBeenCalled();
  });

  it("should not effect other functions", function() {
    expect(bar).toEqual(123);
  });

  it("when called returns the requested value", function() {
    expect(fetchedBar).toEqual(745);
  });
});
```

### and.callFake()

与 returnValue 相似，callFake 则更进一步，直接通过指定一个假的自定义函数来执行。这种方式比 returnValue 更灵活，我们可以任意捏造一个函数来达到我们的测试要求。

```
spyOn(foo, "getBar").and.callFake(function() {
    return 1001;
});
```

### and.throwError()

模拟异常的抛出。

```
describe("A spy, when configured to throw an error", function() {
  var foo, bar;

  beforeEach(function() {
    foo = {
      setBar: function(value) {
        bar = value;
      }
    };

    spyOn(foo, "setBar").and.throwError("quux");
  });

  it("throws the value", function() {
    expect(function() {
        foo.setBar(123)
    }).toThrowError("quux");
  });
});
```
### and.stub()

重置为原始状态

### 其他追踪属性

calls：对于被 spy 的函数的调用，都可以在 calls 属性中跟踪。
+ .calls.any(): 被 spy 的函数一旦被调用过，则返回 true，否则为 false；
+ .calls.count(): 返回被 spy 的函数的被调用次数；
+ .calls.argsFor(index): 返回被 spy 的函数的调用参数，以 index 来指定参数；
+ .calls.allArgs():返回被 spy 的函数的所有调用参数；
+ .calls.all(): 返回 calls 的上下文，这将返回当前 calls 的整个实例数据；
+ .calls.mostRecent(): 返回 calls 中追踪的最近一次的请求数据；
+ .calls.first(): 返回 calls 中追踪的第一次请求的数据；
+ .object: 当调用 all()，mostRecent()，first() 方法时，返回对象的 object 属性返回的是当前上下文对象；
+ .calls.reset(): 重置 spy 的所有追踪数据；

### createSpy

假如没有函数可以追踪，我们可以自己创建一个空的 Spy。创建后的 Spy 功能与其他的 Spy 一样：跟踪调用、参数等，但该 Spy 没有实际的代码实现，这种方式经常会用在对 JavaScript 中的对象的测试。

```
describe("A spy, when created manually", function() {
  var whatAmI;

  beforeEach(function() {
    whatAmI = jasmine.createSpy('whatAmI');
    console.log(whatAmI);
    whatAmI("I", "am", "a", "spy");
  });

  it("is named, which helps in error reporting", function() {
    expect(whatAmI.and.identity).toEqual('whatAmI');
  });

  it("tracks that the spy was called", function() {
    expect(whatAmI).toHaveBeenCalled();
  });

  it("tracks its number of calls", function() {
    expect(whatAmI.calls.count()).toEqual(1);
  });

  it("tracks all the arguments of its calls", function() {
    expect(whatAmI).toHaveBeenCalledWith("I", "am", "a", "spy");
  });

  it("allows access to the most recent call", function() {
    expect(whatAmI.calls.mostRecent().args[0]).toEqual("I");
  });
});
```

### createSpyObj

如果需要 spy 模拟多个函数调用，可以向 jasmine.createSpyObj 中传入一个字符串数组，它将返回一个对象，你所传入的所有字符串都将对应一个属性，每个属性即为一个spy。

```
describe("Multiple spies, when created manually", function() {
  var tape;

  beforeEach(function() {
    tape = jasmine.createSpyObj('tape', ['play', 'pause', 'stop', 'rewind']);
    console.log(tape);
    tape.play();
    tape.pause();
    tape.rewind(0);
  });

  it("creates spies for each requested function", function() {
    expect(tape.play).toBeDefined();
    expect(tape.pause).toBeDefined();
    expect(tape.stop).toBeDefined();
    expect(tape.rewind).toBeDefined();
  });

  it("tracks that the spies were called", function() {
    expect(tape.play).toHaveBeenCalled();
    expect(tape.pause).toHaveBeenCalled();
    expect(tape.rewind).toHaveBeenCalled();
    expect(tape.stop).not.toHaveBeenCalled();
  });

  it("tracks all the arguments of its calls", function() {
    expect(tape.rewind).toHaveBeenCalledWith(0);
  });
});
```

### 异步支持

Jasmine 可以支持 spec 中执行异步操作，当调用 beforeEach, it 和 afterEach 时，函数可以包含一个可选参数 done，当 spec 执行完毕之后，调用 done 通知Jasmine 异步操作已执行完毕。

```
describe("Asynchronous specs", function() {
    var value;
  
    beforeEach(function(done) {
        setTimeout(function() {
            value = 0;
            done();
        }, 1);
    });
  
    // 在上面 beforeEach 的 done() 被执行之前，这个测试用例不会被执行
    it("should support async execution of test preparation and expectations", function(done) {
        value++; 
        expect(value).toBeGreaterThan(0);
        done(); // 执行完 done() 之后，该测试用例真正执行完成
    });
  
    // Jasmine 异步执行超时时间默认为 5 秒，超过后将报错
    describe("long asynchronous specs", function() {
      
        // 如果要调整指定用例的默认的超时时间，可以在 beforeEach，it 和 afterEach 中传入一个时间参数
        beforeEach(function(done) {
            // setTimeout(function(){}, 2000); // 可以试试如果该方法执行超过1秒时js会报错
            done();
        }, 1000);

        it("takes a long time", function(done) {
            setTimeout(function() {
                done();
            }, 9000);
        }, 10000);

        afterEach(function(done) {
            done();
        }, 1000);
    });
});
```

如果需要设置全局的默认超时时间，可以设置 jasmine.DEFAULT_TIMEOUT_INTERVAL 的值。

# VSCode 插件推荐

shark-extension

# Angular 测试工具集

## 普通类的测试

对于普通类比如服务和管道，可以直接通过 new 创建实例进行测试。

```
describe('ValueService', () => {
  	let service: ValueService;

  	beforeEach(() => { 
  		service = new ValueService(); 
  	});

  	it('#getValue should return real value', () => {
  	    expect(service.getValue()).toBe('real value');
  	});

  	it('#getObservableValue should return value from observable', (done: DoneFn) => {
  	  	service.getObservableValue().subscribe(value => {
  			expect(value).toBe('observable value');
  			done();
  	  	});
  	});

  	it('#getPromiseValue should return value from a promise', (done: DoneFn) => {
  	    service.getPromiseValue().then(value => {
  	  		expect(value).toBe('promise value');
  	  		done();
  	    });
  	});
});
```

## Angular TestBed

TestBed 是 Angular 测试工具集提供的用于构建一个 **@NgModule** 测试环境。使用 **TestBed.configureTestingModule()** 来构建测试模块，它接受一个元数据对象，其中具有 @NgModule 中的绝大多数属性。

### 服务测试

要测试某个服务，就要在 providers 中指定一个将要进行测试或模拟的相关服务的数组。

使用 TestBed.inject() 将服务注入到一个测试类

```
describe('ValueService', () => {

    let service: ValueService;

    beforeEach(() => {
        TestBed.configureTestingModule({ 
            providers: [ ValueService ] 
        });
        // 通过调用 TestBed.inject()（参数为该服务类）把它注入到一个测试中。
        service = TestBed.inject(ValueService);
    });

    it('should use ValueService', () => {
        // service = TestBed.inject(ValueService);
        expect(service.getValue()).toBe('real value');
    });

    // 使用 Angular 提供的 async() 进行异步测试
    it('test should wait for ValueService.getPromiseValue', async(() => {
        service.getPromiseValue().then(
            value => expect(value).toBe('promise value')
        );
    }));

    it('test should wait for ValueService.getObservableValue', async(() => {
        service.getObservableValue().subscribe(
            value => expect(value).toBe('observable value')
        );
    }));

    it('test should wait for ValueService.getObservableDelayValue', (done: DoneFn) => {
        service.getObservableDelayValue().subscribe(value => {
            expect(value).toBe('observable delay value');
            done();
        });
    });

    // 使用 fakeAsync() 和 tick();
    it('should allow the use of fakeAsync', fakeAsync(() => {
        let value: any;
        service.getPromiseValue().then((val: any) => value = val);
        // Trigger JS engine cycle until all promises resolve.
        tick(); 
        expect(value).toBe('promise value');
    }));
});
```

在 Sepcs (it) 中使用 inject：

```
describe('use inject within `it`', () => {
    beforeEach(() => {
      TestBed.configureTestingModule({ providers: [ValueService] });
    });

    it('should use modified providers', 
        inject([ValueService], (service: ValueService) => {
            service.setValue('value modified in beforeEach');
            expect(service.getValue()).toBe('value modified in beforeEach');
        })
    );
});
```

对于异步测试时，可以直接在 beforeEach() 中使用 async()：

```
describe('using async(inject) within beforeEach', () => {
    let serviceValue: string;

    beforeEach(() => {
        TestBed.configureTestingModule({ providers: [ValueService] });
    });

    beforeEach(async(inject([ValueService], (service: ValueService) => {
        service.getPromiseValue().then(value => serviceValue = value);
    })));

    it('should use asynchronously modified value ... in synchronous test', () => {
        expect(serviceValue).toBe('promise value');
    });
});
```

### 组件测试

测试环境中创建一个组件，以 DashboardComponent 为例子：


```
describe('DashboardComponent test', () => {
  	let component: DashboardComponent;
	let fixture: ComponentFixture<DashboardComponent>;
	let heroServiceSpy: jasmine.SpyObj<any>;

  	beforeEach(async(() => {
		const spy = jasmine.createSpyObj('HeroService', ['getHeroes']);
			
  	  	TestBed.configureTestingModule({
  	  	  	imports: [
  	  	    	HttpClientTestingModule
  	  	  	],
  	  	  	declarations: [DashboardComponent],
  	  	  	providers: [
  	  	    	{provide: HeroService, useValue: spy},
  	  	  	],
		}).compileComponents();

		heroServiceSpy = TestBed.inject(HeroService);
		heroServiceSpy.getHeroes.and.callFake(() => {
			const sheroes = [
				{ id: 11, name: 'Dr Nice' },
        		{ id: 12, name: 'Narco' },
        		{ id: 13, name: 'Bombasto' },
        		{ id: 14, name: 'Celeritas' }
			];
			return of(sheroes);
		});
  	}));

	  
	beforeEach(() => {
		fixture = TestBed.createComponent(DashboardComponent);
		component = fixture.componentInstance;
		fixture.detectChanges();
		
	});
  
	it('should create', () => {
		expect(fixture).toBeDefined();
  	    expect(component).toBeDefined();
	});

	it('should have <h3> with "Top Heroes"', () => {
		const h3De: DebugElement = fixture.debugElement;
  		const h3El: HTMLElement = h3De.nativeElement;
  		const p = h3El.querySelector('h3');
  		expect(p.textContent).toEqual('Top Heroes');
	});

	it('should find the <h3> with fixture.debugElement.query(By.css)', () => {
		const bannerDe: DebugElement = fixture.debugElement;
		const paragraphDe = bannerDe.query(By.css('h3'));
		const p: HTMLElement = paragraphDe.nativeElement;
		expect(p.textContent).toEqual('Top Heroes');
	});

  	afterEach(() => {
  	  	TestBed.resetTestingModule();
  	});
});
```
+ 配置 TestBed 环境，对于带有外部引用（样式、模板）的组件来说初始化时需要调用 compileComponents() 进行编译。由于 compileComponents 是异步的，所以要使用 async() 函数处理
+ 置好 TestBed 之后，调用它的 createComponent() 方法，它会创建一个 DashboardComponent 的实例，把相应的元素添加到测试运行器的 DOM 中，然后返回一个 ComponentFixture 对象。ComponentFixture 用来与所创建的组件及其 DOM 元素进行交互。
+ 使用 fixture.componentInstance 来访问组件实例。
+ 获取组件元素 nativeElement 和 DebugElement。 前者原生 DOM 元素，属性取决于运行环境，后者是由 Angular 进行包装可以安全的横跨其支持的所有平台运行并提供诸如 query 或者 triggerEventHandler 等。
+ 使用 By 类支持跨平台应用。
+ TestBed.createComponent 不能触发变更检测，使用 detectChanges() 触发检查。

### HTTP 测试

Angular 提供了 HttpClientTestingModule 用来测试。

```
// Http testing module and mocking controller
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';

// Other imports
import { TestBed } from '@angular/core/testing';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { HttpHeaders } from '@angular/common/http';
interface Data {
    name: string;
}
const testUrl = '/data';
describe('HttpClient testing', () => {
    let httpClient: HttpClient;
    let httpTestingController: HttpTestingController;

    beforeEach(() => {
        TestBed.configureTestingModule({
            imports: [ HttpClientTestingModule ]
        });

        // 调用 TestBed.inject()，来获取注入的 HttpClient 服务和模拟对象的控制器 HttpTestingController，以便在测试期间引用它们。
        httpClient = TestBed.inject(HttpClient);
        httpTestingController = TestBed.inject(HttpTestingController);
    });

    afterEach(() => {
      // 在每次测试之后，断言不再有挂起的请求
      httpTestingController.verify();
    });

    it('can test HttpClient.get', () => {
        const testData: Data = {name: 'Test Data'};
        // 模拟发起一个 HTTP GET 请求
        httpClient.get<Data>(testUrl).subscribe(data => {
            console.log(data);
            expect(data).toEqual(testData)
        });
          
        // 使用 expectOne() 匹配请求的 URL
        // 如果匹配到多个或者没有匹配到，则后抛出失败
        const req = httpTestingController.expectOne('/data');
        // 断言 request 是不是 GET 方法
        expect(req.request.method).toEqual('GET');
        // 使用模拟数据进行响应
        req.flush(testData);
    });

    it('can test HttpClient.get with matching header', () => {
        const testData: Data = {name: 'Test Data'};
        // 发起一个请求设置指定的 header
        httpClient.get<Data>(testUrl, {
            headers: new HttpHeaders({'Authorization': 'my-auth-token'})
        }).subscribe(data =>
            expect(data).toEqual(testData)
        );
        const req = httpTestingController.expectOne(
            req => req.headers.has('Authorization')
        );
        req.flush(testData);
    });

    it('can test for 404 error', () => {
        const emsg = 'deliberate 404 error';
        httpClient.get<Data[]>(testUrl).subscribe(
            data => fail('should have failed with the 404 error'),
            (error: HttpErrorResponse) => {
                console.log(error);
                expect(error.status).toEqual(404);
                expect(error.error).toEqual(emsg);
            }
        );
        const req = httpTestingController.expectOne(testUrl);
        // Respond with mock error
        req.flush(emsg, { status: 404, statusText: 'Not Found' });
    });
});
```


## 遗留问题

1. Jasmine 执行测试用例是否是并行的？  不是，angular 是串行执行的

下面代码的实际运行时间为 4s。
```
describe("first suite", function() { 
    it("first test", function(done) { 
    	expect(true).toBeTruthy(); 
    	setTimeout(done, 1000); 
    }); 

    it("second test", function(done) { 
    	expect(true).toBeTruthy(); 
    	setTimeout(done, 1000); 
    }); 
}); 

describe("second suite", function() { 
    it("first test", function(done) { 
    	expect(true).toBeTruthy(); 
    	setTimeout(done, 1000); 
    }); 

    it("second test", function(done) { 
    	expect(true).toBeTruthy(); 
    	setTimeout(done, 1000); 
    }); 
});

// Chrome 80.0.3987 (Windows 10.0.0): Executed 4 of 4 SUCCESS (4.034 secs / 4.025 secs)
```
